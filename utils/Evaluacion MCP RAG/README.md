# Evaluación: MCP Electoral vs RAG Baseline — Resultados completos

**Corrida**: `full-final` · **Fecha**: 2026-05-23 · **Rama git**: `evaluation`  
**Modelo respuestas**: Claude Sonnet (Claude Code CLI 2.1.150) · **Modelo juez**: DeepSeek-chat (temp=0.2)  
**Golden set hash**: `b4815141be39`

---

## Contenido

1. [Objetivo](#1-objetivo)
2. [Diseño experimental](#2-diseño-experimental)
3. [Métricas implementadas](#3-métricas-implementadas)
4. [Resultados cuantitativos](#4-resultados-cuantitativos)
5. [Análisis de trazas de herramientas](#5-análisis-de-trazas-de-herramientas)
6. [LLM-as-judge: resultados completos (30/30)](#6-llm-as-judge-resultados-completos-3030)
7. [Ejemplos cualitativos](#7-ejemplos-cualitativos)
8. [Discusión](#8-discusión)
9. [Limitaciones y trabajo futuro](#9-limitaciones-y-trabajo-futuro)
10. [Reproducibilidad](#10-reproducibilidad)

---

## 1. Objetivo

Este experimento compara dos estrategias para responder preguntas sobre las elecciones presidenciales del Perú 2026:

| Sistema | Descripción |
|---|---|
| **MCP Electoral** | Servidor FastMCP con 14 tools especializadas sobre Postgres + Neo4j. Claude accede a datos estructurados en tiempo real vía llamadas a herramientas. |
| **RAG Baseline** | Recuperación por similitud semántica (cosine, pgvector) sobre `rag_chunks` (1 893 fragmentos). Claude recibe los 5 chunks más relevantes como contexto; las tools del MCP están bloqueadas. |

La hipótesis central es que el acceso estructurado y orientado a herramientas permite respuestas más exactas, completas y actualizadas que la recuperación de texto por similitud, especialmente en preguntas sobre datos tabulares (encuestas, macroeconomía).

---

## 2. Diseño experimental

### 2.1 Golden set

- **30 preguntas** con ground truth validado contra la base de datos real (graph_version `2026-03-13`, cobertura mediática 10–12 de marzo de 2026, encuestas Datum enero–abril 2026).
- **4 tipos**:

| Tipo | N | Descripción |
|---|---|---|
| `factual` | 10 | Dato preciso: porcentaje en encuesta, valor IGBVL, tipo de cambio. |
| `exploratorio` | 10 | Resumen de tendencias: temas del momento, cobertura por medio, sentimiento. |
| `causal` | 5 | Análisis de causa-efecto: correlación evento-encuesta, variación macro. |
| `edge_case` | 5 | Datos inexistentes (Ipsos, candidato no registrado) o ambiguos. |

- Las preguntas fueron diseñadas para cubrir las **14 tools** del MCP; cada pregunta incluye `tools_esperadas` (herramientas que debería seleccionar el modelo).

### 2.2 Pipeline MCP

```
Claude Sonnet (--print)
  └─ allowedTools: mcp__electoral-peru-2026__*
       └─ FastMCP 3.2.4 / streamable-http / localhost:8080
            ├─ asyncpg → PostgreSQL (encuestas, macro, candidatos)
            └─ neo4j driver → Neo4j (grafo mediático)
```

- El modelo selecciona las herramientas libremente; puede encadenarlas en múltiples turnos.
- Se capturan los `tool_result` de cada llamada para el juez de faithfulness.

### 2.3 Pipeline RAG Baseline

```
Claude Sonnet (--print)
  └─ disallowedTools: mcp__*, Bash, Read, Write, Edit, ...
  └─ strict-mcp-config (bloquea MCPs del config global)
       └─ prompt usuario = pregunta + 5 chunks recuperados por cosine
            └─ pgvector / paraphrase-multilingual-mpnet-base-v2
```

- **Nota metodológica crítica**: el flag `--disallowedTools 'mcp__*'` no es suficiente para bloquear MCPs registrados globalmente en `~/.claude.json`. Se requiere `--strict-mcp-config`, que ignora toda configuración MCP global. Una corrida preliminar sin este flag resultó en que el RAG invocaba tools del MCP. La corrida `full-final` usa la corrección. Los 30 RAG responses tienen `n_tool_calls = 0`, confirmando el aislamiento.

### 2.4 Motor de evaluación

Ambos pipelines se ejecutan con `claude --print --output-format stream-json` desde scripts Python. No se requiere `ANTHROPIC_API_KEY`; las llamadas usan el plan Max del usuario. El script `eval_judge.py` implementa el LLM-as-judge vía **DeepSeek API** (httpx directo, `deepseek-chat`, temperatura 0.2, JSON mode forzado). Se usa un modelo distinto al que generó las respuestas para evitar sesgo de auto-evaluación.

---

## 3. Métricas implementadas

| Métrica | Tipo | Descripción | Estado |
|---|---|---|---|
| **Embedding similarity** | Determinística | Similitud coseno entre respuesta y ground truth (mpnet-multilingual). | ✅ 30/30 |
| **Tool selection accuracy** | Determinística | ¿El modelo eligió las herramientas esperadas (ignorando ToolSearch)? | ✅ 30/30 |
| **Latencia (ms)** | Determinística | Tiempo de ejecución de cada invocación CLI. | ✅ 30/30 |
| **Faithfulness** | LLM-as-judge | Claims de la respuesta sustentadas por el contexto. | ✅ 30/30 |
| **Answer correctness** | LLM-as-judge | Cobertura de los elementos del ground truth. | ✅ 30/30 |
| **Answer relevancy** | LLM-as-judge | ¿La respuesta atiende la pregunta? | ✅ 30/30 |

El juez usa DeepSeek-chat (temperatura 0.2) vía API directa con httpx. Las 180 invocaciones (30×3×2 pipelines) completaron en ~15 minutos.

---

## 4. Resultados cuantitativos

### 4.1 Métricas globales de corrida

| Métrica | MCP | RAG |
|---|---|---|
| Preguntas evaluadas | 30 | 30 |
| Errores | 0 | 0 |
| Avg. latencia (ms) | 30 199 | 11 547 |
| Avg. tool calls / pregunta | 2.9 | 0 |
| Costo total estimado (USD) | 2.80 | 1.63 |
| Chunks / pregunta | — | 5 (fijo) |

El MCP es **2.6× más lento** y **1.7× más caro** que el RAG, dado que realiza múltiples llamadas a herramientas en lugar de una sola recuperación semántica. Este trade-off se evalúa frente a la ganancia en calidad de respuesta.

### 4.2 Similitud coseno (embedding similarity) — 30/30 preguntas

Métrica determinística: similitud coseno entre la respuesta del sistema y el ground truth del golden set, usando `paraphrase-multilingual-mpnet-base-v2`.

| Tipo | MCP (media) | RAG (media) | Δ | MCP gana |
|---|---|---|---|---|
| **factual** (n=10) | 0.706 | 0.567 | +0.139 | 9/10 |
| **exploratorio** (n=10) | 0.735 | 0.606 | +0.129 | 7/10 |
| **causal** (n=5) | 0.704 | 0.538 | +0.166 | 4/5 |
| **edge_case** (n=5) | 0.738 | 0.588 | +0.150 | 5/5 |
| **Global** (n=30) | **0.721** | **0.579** | **+0.142** | **25/30** |

> **El MCP supera al RAG en 25 de 30 preguntas** (83.3%) en similitud de embeddings. La ventaja es especialmente pronunciada en preguntas `edge_case` (+0.150) y `causal` (+0.166).

#### Tabla completa por pregunta

| ID | Tipo | MCP emb | RAG emb | Δ | Ganador |
|---|---|---|---|---|---|
| F01 | factual | 0.783 | 0.824 | −0.042 | RAG |
| F02 | factual | 0.587 | 0.500 | +0.086 | MCP |
| F03 | factual | 0.637 | 0.791 | −0.154 | RAG |
| F04 | factual | 0.710 | 0.405 | +0.304 | MCP |
| F05 | factual | 0.619 | 0.414 | +0.205 | MCP |
| F06 | factual | 0.818 | 0.606 | +0.212 | MCP |
| F07 | factual | 0.770 | 0.374 | +0.396 | MCP |
| F08 | factual | 0.759 | 0.554 | +0.205 | MCP |
| F09 | factual | 0.797 | 0.719 | +0.078 | MCP |
| F10 | factual | 0.583 | 0.486 | +0.097 | MCP |
| E01 | exploratorio | 0.739 | 0.692 | +0.047 | MCP |
| E02 | exploratorio | 0.760 | 0.480 | +0.281 | MCP |
| E03 | exploratorio | 0.701 | 0.781 | −0.080 | RAG |
| E04 | exploratorio | 0.855 | 0.521 | +0.334 | MCP |
| E05 | exploratorio | 0.760 | 0.778 | −0.018 | RAG |
| E06 | exploratorio | 0.759 | 0.670 | +0.088 | MCP |
| E07 | exploratorio | 0.802 | 0.657 | +0.145 | MCP |
| E08 | exploratorio | 0.531 | 0.529 | +0.002 | MCP |
| E09 | exploratorio | 0.803 | 0.630 | +0.172 | MCP |
| E10 | exploratorio | 0.637 | 0.324 | +0.313 | MCP |
| C01 | causal | 0.793 | 0.638 | +0.154 | MCP |
| C02 | causal | 0.508 | 0.567 | −0.059 | RAG |
| C03 | causal | 0.747 | 0.665 | +0.082 | MCP |
| C04 | causal | 0.622 | 0.420 | +0.201 | MCP |
| C05 | causal | 0.850 | 0.401 | +0.450 | MCP |
| X01 | edge_case | 0.621 | 0.447 | +0.173 | MCP |
| X02 | edge_case | 0.750 | 0.644 | +0.106 | MCP |
| X03 | edge_case | 0.760 | 0.674 | +0.085 | MCP |
| X04 | edge_case | 0.798 | 0.717 | +0.082 | MCP |
| X05 | edge_case | 0.762 | 0.457 | +0.304 | MCP |

Las 5 preguntas donde el RAG supera al MCP (F01, F03, E03, E05, C02) se analizan en la sección de discusión.

### 4.3 Latencia por tipo de pregunta (MCP)

| Tipo | Avg ms | Min ms | Max ms |
|---|---|---|---|
| factual | 25 991 | 13 711 | 68 126 |
| exploratorio | 26 332 | 14 885 | 36 413 |
| causal | 54 577 | 23 722 | 82 206 |
| edge_case | 21 972 | 15 294 | 29 228 |

Las preguntas `causal` requieren el mayor tiempo porque el modelo encadena más herramientas (hasta 7 llamadas en C01).

---

## 5. Análisis de trazas de herramientas

### 5.1 Resumen de actividad

| Métrica | Valor |
|---|---|
| Tool calls totales (30 preguntas) | 87 |
| ToolSearch (descubrimiento de API) | 35 (40.2%) |
| Calls reales al MCP electoral | 52 (59.8%) |
| Promedio real MCP calls / pregunta | 1.7 |
| Tools distintas del MCP utilizadas | 13/14 |

`ToolSearch` es una meta-herramienta del CLI de Claude Code que permite al modelo descubrir el schema de las tools del MCP antes de invocarlas. No representa una llamada al servidor MCP. El patrón `ToolSearch → mcp__*` se repite en el 100% de las preguntas.

### 5.2 Frecuencia de uso por herramienta

| Tool MCP | Llamadas | % del total real |
|---|---|---|
| `buscar_tema_por_descripcion` | 7 | 13.5% |
| `citar_evidencia` | 7 | 13.5% |
| `intencion_voto_candidato` | 6 | 11.5% |
| `contexto_candidato` | 6 | 11.5% |
| `encuestas_disponibles` | 4 | 7.7% |
| `temas_top_periodo` | 4 | 7.7% |
| `panorama_diario` | 3 | 5.8% |
| `tipo_cambio_periodo` | 3 | 5.8% |
| `indicador_macro_periodo` | 3 | 5.8% |
| `buscar_actor_por_descripcion` | 3 | 5.8% |
| `cobertura_por_medio` | 2 | 3.8% |
| `evolucion_sentimiento_candidato` | 2 | 3.8% |
| `comparar_candidatos_sentimiento` | 1 | 1.9% |
| `noticias_por_actor` | 1 | 1.9% |

Las tools no utilizadas (`comparar_candidatos_sentimiento`, `noticias_por_actor`) se invocan en preguntas `causal` complejas. La única tool no invocada en esta corrida es ninguna — **las 14 tools del MCP fueron seleccionadas al menos una vez**.

### 5.3 Encadenamientos más frecuentes

El patrón más común es un pipeline de dos pasos: descubrimiento semántico seguido de recuperación estructurada.

| Secuencia | Frecuencia | Interpretación |
|---|---|---|
| `ToolSearch → buscar_tema_por_descripcion` | 6 | Localizar topic antes de buscar evidencia |
| `ToolSearch → intencion_voto_candidato` | 5 | Consulta directa de encuestas |
| `buscar_tema_por_descripcion → citar_evidencia` | 4 | Identificar tema → recuperar citas del grafo |
| `ToolSearch → encuestas_disponibles` | 3 | Explorar qué encuestas existen |
| `citar_evidencia → citar_evidencia` | 3 | Iterar sobre múltiples temas |

### 5.4 Precisión en selección de herramientas

Se evaluó si el modelo seleccionó las `tools_esperadas` del golden set, ignorando `ToolSearch` (herramienta de descubrimiento no esperada):

| Resultado | N | % |
|---|---|---|
| **Match completo** (todas las esperadas, puede haber extras) | 25 | 83.3% |
| **Match parcial** (al menos una esperada) | 5 | 16.7% |
| **Sin match** | 0 | 0.0% |
| **Al menos una match (>=partial)** | **30** | **100%** |

El modelo **siempre seleccionó al menos una de las herramientas esperadas**. Los 5 casos de match parcial corresponden a situaciones donde el modelo sustituyó o omitió una herramienta prevista pero usó otra relacionada (ej. C03: usó `buscar_tema_por_descripcion` en lugar de `temas_top_periodo` para la parte mediática de la pregunta).

---

## 6. LLM-as-judge: resultados completos (30/30)

Juez: DeepSeek-chat, temperatura 0.2, JSON mode. Implementa el esquema Ragas adaptado (faithfulness, answer_correctness, answer_relevancy).

### 6.1 Resumen global

| Métrica | MCP | RAG | Δ | Ganador |
|---|---|---|---|---|
| **Faithfulness** | 0.854 | 0.868 | −0.014 | RAG (marginal) |
| **Answer correctness** | **0.792** | **0.196** | **+0.596** | **MCP** |
| **Answer relevancy** | **0.842** | **0.388** | **+0.454** | **MCP** |
| **Embedding similarity** | **0.721** | **0.579** | **+0.142** | **MCP** |

> El RAG logra faithfulness ligeramente mayor (0.868 vs 0.854) porque es "fiel" a sus chunks — aunque esos chunks contengan datos desactualizados o incompletos. La diferencia crítica está en **answer_correctness** (+0.596): el MCP accede a la base de datos estructurada y devuelve los datos correctos; el RAG recupera texto que a menudo no contiene la respuesta o contiene una versión anterior.

### 6.2 Por tipo de pregunta

| Tipo | Métrica | MCP | RAG | Δ |
|---|---|---|---|---|
| **factual** | faithfulness | 0.732 | 0.822 | −0.090 |
| | answer_correctness | **0.980** | **0.078** | **+0.902** |
| | answer_relevancy | 0.985 | 0.460 | +0.525 |
| **exploratorio** | faithfulness | 0.935 | 0.881 | +0.054 |
| | answer_correctness | **0.613** | **0.115** | **+0.498** |
| | answer_relevancy | 0.895 | 0.385 | +0.510 |
| **causal** | faithfulness | 0.923 | 0.871 | +0.052 |
| | answer_correctness | **0.830** | **0.350** | **+0.480** |
| | answer_relevancy | 0.950 | 0.480 | +0.470 |
| **edge_case** | faithfulness | 0.867 | 0.933 | −0.066 |
| | answer_correctness | **0.733** | **0.438** | **+0.295** |
| | answer_relevancy | 0.340 | 0.160 | +0.180 |

**Notas por tipo:**
- **Factual**: la brecha más dramática en correctness (+0.902). El RAG casi nunca tiene el dato exacto actualizado (encuesta correcta, valor macro del día preciso).
- **Edge_case**: answer_relevancy baja en ambos (MCP=0.34, RAG=0.16) porque las preguntas sobre datos inexistentes generan respuestas de rechazo que el juez evalúa como poco "relevantes" — comportamiento correcto del sistema, artefacto de la métrica.
- **Exploratorio**: el MCP tiene faithfulness alta (0.935) con datos de tool_results verificables. El RAG tiene correctness cercana a cero (0.115) porque sus chunks no cubren el rango temporal o la granularidad requerida.

### 6.3 Tabla completa por pregunta

| ID | Tipo | MCP faith | MCP corr | MCP relev | RAG faith | RAG corr | RAG relev |
|---|---|---|---|---|---|---|---|
| F01 | factual | 0.875 | 1.00 | 1.00 | 0.750 | 0.00 | 0.90 |
| F02 | factual | 1.000 | 1.00 | 1.00 | 0.667 | 0.00 | 0.20 |
| F03 | factual | 1.000 | 0.80 | 1.00 | 0.833 | 0.20 | 1.00 |
| F04 | factual | 0.500 | 1.00 | 1.00 | 1.000 | 0.00 | 0.20 |
| F05 | factual | 0.750 | 1.00 | 0.90 | 1.000 | 0.33 | 0.20 |
| F06 | factual | 0.625 | 1.00 | 1.00 | 0.750 | 0.00 | 0.20 |
| F07 | factual | 1.000 | 1.00 | 1.00 | 1.000 | 0.00 | 0.00 |
| F08 | factual | 0.857 | 1.00 | 1.00 | 0.800 | 0.00 | 1.00 |
| F09 | factual | 0.714 | 1.00 | 1.00 | 0.750 | 0.25 | 0.90 |
| F10 | factual | 0.000 | 1.00 | 0.95 | 0.667 | 0.00 | 0.00 |
| E01 | exploratorio | 0.870 | 0.77 | 0.95 | — | 0.00 | 0.95 |
| E02 | exploratorio | 1.000 | 0.80 | 1.00 | 0.600 | 0.00 | 0.20 |
| E03 | exploratorio | 1.000 | 0.00 | 0.20 | 1.000 | 0.00 | 0.20 |
| E04 | exploratorio | 1.000 | 0.50 | 1.00 | 0.333 | 0.00 | 0.00 |
| E05 | exploratorio | 1.000 | 1.00 | 0.95 | 1.000 | 0.00 | 0.00 |
| E06 | exploratorio | 0.667 | 1.00 | 1.00 | 1.000 | 0.00 | 0.20 |
| E07 | exploratorio | 1.000 | 0.83 | 1.00 | 1.000 | 0.75 | 0.95 |
| E08 | exploratorio | 0.938 | 0.40 | 0.90 | 1.000 | 0.40 | 0.95 |
| E09 | exploratorio | 1.000 | 0.50 | 1.00 | 1.000 | 0.00 | 0.20 |
| E10 | exploratorio | 0.875 | 0.33 | 0.95 | 1.000 | 0.00 | 0.20 |
| C01 | causal | 0.893 | 1.00 | 0.95 | 0.857 | 0.43 | 0.90 |
| C02 | causal | 1.000 | 0.65 | 0.95 | 1.000 | 0.00 | 0.20 |
| C03 | causal | 1.000 | 1.00 | 0.95 | 1.000 | 0.67 | 0.20 |
| C04 | causal | 0.727 | 0.50 | 0.90 | 0.500 | 0.40 | 0.80 |
| C05 | causal | 1.000 | 1.00 | 1.00 | 1.000 | 0.25 | 0.30 |
| X01 | edge_case | 1.000 | 1.00 | 0.20 | 1.000 | 1.00 | 0.20 |
| X02 | edge_case | 1.000 | 0.60 | 0.20 | 1.000 | 0.33 | 0.00 |
| X03 | edge_case | 1.000 | 0.67 | 0.20 | 1.000 | 0.33 | 0.20 |
| X04 | edge_case | 1.000 | 0.40 | 0.20 | 1.000 | 0.33 | 0.20 |
| X05 | edge_case | 0.333 | 1.00 | 0.90 | 0.667 | 0.20 | 0.20 |

---

## 7. Ejemplos cualitativos

### 7.1 F01 — Dato de encuesta (MCP correcto, RAG desactualizado)

**Pregunta**: ¿Cuál fue el porcentaje de Keiko Fujimori en la última encuesta Datum disponible?

| | MCP | RAG |
|---|---|---|
| **Respuesta** | "Keiko Fujimori: **14.5%** (encuesta del **5 de abril de 2026**), posición #1, n=3000, ±1.8pp" | "10.7% según fragmento YouTube del **10 de marzo de 2026**. No puedo confirmar si es la última encuesta." |
| **Embedding sim** | 0.783 | 0.824 |
| **Correctness** | 1.00 | 0.00 |
| **Análisis** | La herramienta `intencion_voto_candidato` devuelve la serie completa de 9 encuestas. El modelo identifica correctamente la más reciente. | Los chunks recuperados solo cubren hasta el 10-11 de marzo. El modelo reporta la encuesta de ese día como si fuera la última. |

> Este caso demuestra la ventaja fundamental del MCP sobre RAG para datos tabulares con timestamp: la base de datos tiene la serie completa; el índice vectorial solo ve el texto disponible al momento de la indexación.

---

### 7.2 F07 — Dato macro IGBVL (MCP responde, RAG sin chunks relevantes)

**Pregunta**: ¿Cuál fue el día con mayor caída de la Bolsa de Valores de Lima (IGBVL) en 2026?

| | MCP | RAG |
|---|---|---|
| **Respuesta** | "El **30 de enero de 2026** con -5.05%. Top 5 caídas: 30-ene (-5.05%), 15-abr (-4.48%), 03-mar (-3.88%)..." | "Los fragmentos recuperados **no contienen información sobre el IGBVL**. Los textos tratan sobre gas Camisea, oleajes y tipo de cambio." |
| **Embedding sim** | 0.770 | 0.374 |
| **Análisis** | `indicador_macro_periodo` devuelve la serie diaria del IGBVL desde Postgres. Respuesta precisa y completa. | Sin noticias sobre la BVL en el corpus, el RAG no puede responder. El modelo declara la limitación correctamente. |

> Ilustra el límite estructural del RAG: si el dato no apareció en noticias indexadas, no puede recuperarse. El MCP accede a fuentes estructuradas (BCRP) independientemente de la cobertura mediática.

---

### 7.3 C05 — Tendencia de encuesta a lo largo del tiempo (Δ embedding = +0.450)

**Pregunta**: ¿Cómo evolucionó la intención de voto de Keiko Fujimori desde la primera encuesta hasta la última? Describí la tendencia.

| | MCP | RAG |
|---|---|---|
| **Respuesta** | Tabla completa de 9 encuestas (ene-abr 2026), tendencia al alza de 8.8% → 14.5%, posición en cada fecha. | Solo menciona el dato del 10 de marzo (10.7%). Indica que no tiene la serie completa. |
| **Embedding sim** | 0.850 | 0.401 |

> El mayor delta de toda la corrida (+0.450). La herramienta `intencion_voto_candidato` devuelve la serie temporal completa en una sola call. El RAG no puede construir una serie: los chunks están fragmentados y solo cubren noticias puntuales.

---

### 7.4 X01 — Edge case: dato inexistente (Ipsos, fecha futura)

**Pregunta**: Dame el porcentaje de Keiko Fujimori en la encuesta Ipsos del 15 de mayo de 2026.

| | MCP | RAG |
|---|---|---|
| **Respuesta** | "No tengo esa información. Razones: 1) Ipsos no está integrada (solo Datum). 2) La fecha no existe en el dataset (última encuesta: 5 de abril)." | "Los fragmentos no contienen datos de Ipsos del 15 de mayo. Solo hay datos Datum de marzo. No puedo responder sin inventar." |
| **Embedding sim** | 0.621 | 0.447 |

> Ambos sistemas rechazan correctamente la pregunta sin alucinar. El MCP es más informativo: puede dar las razones precisas (Ipsos no integrada, fecha fuera de rango) porque tiene acceso a los metadatos del sistema. El RAG solo puede decir que no encontró chunks relevantes.

---

### 7.5 C02 — Pregunta comparativa (uno de los 5 casos donde RAG "supera" al MCP)

**Pregunta**: Compara la cobertura mediática (sentimiento) de los 3 candidatos más mencionados en marzo y explica las diferencias.

| | MCP | RAG |
|---|---|---|
| **Embedding sim** | 0.508 | 0.567 |
| **Análisis** | El MCP genera un análisis comparativo usando `comparar_candidatos_sentimiento` + `panorama_diario`. La respuesta es extensa y estructurada pero con latencia de 82s (4 tools, 5 turnos). Embedding ligeramente menor porque el modelo agrega análisis interpretativo que diverge del tono más conciso del ground truth. | El RAG no puede responder la pregunta (sin datos de sentimiento en chunks), pero genera texto fluido que "suena" similar al ground truth en el espacio semántico. |

> Este es el tipo de caso más problemático: el RAG "gana" en embedding similarity no porque su respuesta sea correcta, sino porque declara limitaciones en un tono similar al ground truth. Refuerza la necesidad de métricas LLM-as-judge (answer_correctness) como complemento obligatorio a la similitud cosena.

---

## 8. Discusión

### 8.1 ¿Cuándo el MCP supera al RAG?

El MCP tiene ventaja clara en:

1. **Datos tabulares con timestamp** (encuestas, tipo de cambio, IGBVL): el acceso directo a la base de datos devuelve la serie actualizada completa, mientras que el RAG queda limitado a los fragmentos textuales indexados, que pueden estar desactualizados o incompletos.

2. **Preguntas que requieren múltiples fuentes**: el modelo puede encadenar herramientas (`buscar_tema_por_descripcion → citar_evidencia → evolucion_sentimiento_candidato`) para construir una respuesta integrada.

3. **Rechazos informativos en edge cases**: el MCP puede comunicar no solo "no sé" sino también *por qué* no sabe (encuestadora no integrada, fecha fuera de rango), lo que es más útil para el usuario final.

4. **Preguntas sobre indicadores macroeconómicos**: los datos del BCRP (IGBVL, EMBI, tipo de cambio) no aparecen en noticias con la granularidad necesaria, por lo que el RAG frecuentemente no puede responder.

### 8.2 ¿Cuándo el RAG se defiende o supera al MCP?

En 5 de 30 preguntas el RAG obtiene mayor embedding similarity:

- **F01**: El RAG recupera un fragmento con el dato (aunque desactualizado). El MCP da la respuesta correcta, pero el phrasing más técnico/tabular se aleja del tono del ground truth.
- **F03**: Similar a F01 — el RAG da una respuesta confiada pero incorrecta (10.7% en lugar de 14.5%) que "suena" al ground truth; el MCP usa 6 llamadas y da la respuesta correcta pero más verbosa.
- **E03, E05**: Preguntas exploratorias sobre temas mediáticos donde los chunks contienen exactamente el contenido necesario y el ground truth es de naturaleza descriptiva.
- **C02**: El RAG declara limitaciones en un tono similar al ground truth.

La similitud cosena no captura la dirección del error: un RAG que dice "10.7%" con confianza obtiene mayor embedding similarity que un MCP que dice "14.5%" con tablas y contexto adicional, aunque el MCP sea el correcto. Los resultados del LLM-as-judge confirman esto: en esas 5 preguntas el MCP obtiene mayor `answer_correctness` en 4 de 5 casos, validando que el MCP efectivamente responde mejor a pesar de perder en la métrica de embeddings.

### 8.3 Trade-off latencia/costo vs. calidad

| Dimensión | MCP | RAG |
|---|---|---|
| Latencia avg | 30.2s | 11.5s |
| Costo (30q) | USD 2.80 | USD 1.63 |
| Embedding similarity | 0.721 | 0.579 |
| Answer correctness | **0.792** | **0.196** |
| Answer relevancy | **0.842** | **0.388** |
| Faithfulness | 0.854 | 0.868 |

El MCP es 2.6× más lento y 1.7× más caro, pero entrega 4× más correctness y 2.2× más relevancy. Para una aplicación interactiva de tesis (bajo volumen), la latencia adicional es aceptable dado el salto de calidad. Para producción a escala, habría que evaluar un RAG aumentado con reranking y filtros de fecha.

### 8.4 Precisión de selección de herramientas (83.3%)

El modelo seleccionó las herramientas exactas esperadas en 25/30 preguntas. En los 5 casos de match parcial, la sustitución fue semánticamente razonable (ej. `buscar_tema_por_descripcion` en lugar de `temas_top_periodo` para encontrar el tema antes de listarlo). **Esto sugiere que las docstrings de las tools son suficientemente descriptivas** para que el modelo infiera el uso correcto sin instrucciones adicionales.

---

## 9. Limitaciones y trabajo futuro

### Limitaciones actuales

1. **Single-run de respuestas y juez**: tanto las respuestas como los scores del juez se obtuvieron en una sola corrida. Con LLM no determinísticos (temp > 0) hay varianza; idealmente se promediarían 3 corridas para estimar estabilidad. El juez usa temp=0.2 para reducir varianza manteniendo algo de diversidad.

2. **Cobertura del corpus RAG**: el graph_version cubre 3 días de noticias (10-12 de marzo de 2026). El RAG está en desventaja estructural para preguntas sobre macroeconomía y encuestas posteriores a esa fecha. Una evaluación más justa requeriría un corpus más amplio o restringir las preguntas exclusivamente al período cubierto.

3. **Embedding similarity como proxy imperfecto**: como se documenta en §8.2, la similitud cosena no captura la dirección del error. Las métricas LLM-as-judge son el complemento necesario — y en esta corrida están disponibles para las 30 preguntas.

4. **Golden set pequeño**: 30 preguntas distribuidas en 4 tipos. Los promedios por tipo (`causal` n=5, `edge_case` n=5) tienen alta varianza con muestras tan pequeñas.

### Trabajo futuro

- [x] Corrida completa de LLM-as-judge (30/30 preguntas, DeepSeek-chat).
- [ ] Ampliar corpus RAG con noticias de enero-abril 2026 para una comparación más justa en preguntas temporales.
- [ ] Evaluar un RAG con reranking (cross-encoder) como baseline más fuerte.
- [ ] Múltiples corridas (n=3) para estimar varianza inter-run.
- [ ] Incluir preguntas sobre las otras encuestadoras (Ipsos, CPI) una vez integradas al MCP.

---

## 10. Reproducibilidad

### Archivos en este directorio

| Archivo | Descripción |
|---|---|
| `mcp_responses.json` | 30 respuestas del pipeline MCP con tool_calls y tool_results |
| `rag_responses.json` | 30 respuestas del pipeline RAG baseline con contexts (5 chunks cada una) |
| `manifest_mcp.json` | Versión git, modelo, config al momento de la corrida MCP |
| `manifest_rag.json` | Ídem para RAG |
| `traces_summary.md` | Análisis de tool traces: frecuencias, encadenamientos, duraciones |
| `traces_summary.json` | Datos raw del análisis de trazas |
| `embedding_similarities.json` | Similitud coseno por pregunta para ambos pipelines |

### Scripts de evaluación

```bash
# Reproducir corrida MCP (requiere servidor MCP activo)
python evaluation/scripts/run_mcp.py --name full-final

# Reproducir corrida RAG (requiere rag_chunks indexada)
python evaluation/scripts/run_rag_baseline.py --name full-final

# Analizar trazas
python evaluation/scripts/eval_traces.py \
  --responses evaluation/results/full-final/mcp_responses.json

# Ejecutar juez LLM (requiere DEEPSEEK_API_KEY en .env)
python evaluation/scripts/eval_judge.py \
  --run-dir evaluation/results/full-final
```

### Versiones

| Componente | Versión |
|---|---|
| Claude Code CLI | 2.1.150 |
| Modelo respuestas | Claude Sonnet |
| Modelo juez | DeepSeek-chat (temp=0.2) |
| FastMCP | 3.2.4 |
| Sentence Transformers | paraphrase-multilingual-mpnet-base-v2 |
| Python | 3.12.13 |
| pgvector (RAG chunks) | 1893 chunks indexados |
| Git rev (MCP run) | `e446761fc97a` |
| Git rev (RAG run) | `385769cf0052` |
| Git branch | `evaluation` |
| Golden set hash | `b4815141be39` |

---

*Generado el 2026-05-23. LLM-as-judge completo (30/30) con DeepSeek-chat — evaluación finalizada.*
