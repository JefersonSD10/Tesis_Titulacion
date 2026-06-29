# Prompt — Insertar los cambios en la tabla de correcciones

> Pega el siguiente prompt en una IA para que vuelque esta lista en la tabla de
> correcciones de la tesis. Cambios derivados de los commits `d707020`
> (reformulación), `327bd0c` (renombrado + público objetivo) y `d20416c` /
> `6b203f5` (anexos PNG). Páginas = número impreso (el del índice).

**Prompt:**

Eres un asistente que llena una **tabla de correcciones** de una tesis. La tabla tiene **tres columnas**: la primera, una segunda para el cambio realizado y una tercera para la página. Inserta los registros de abajo siguiendo estas reglas **estrictas**:

1. **Deja SIEMPRE la PRIMERA columna en BLANCO (vacía).** No escribas nada en ella.
2. Llena solo la **segunda columna** con el texto del cambio y la **tercera columna** con su página.
3. Si varios cambios caen en la **misma página**, van **juntos en la misma fila/celda**; no los separes.
4. Respeta el texto y el orden tal cual. No inventes ni agregues filas.

Registros a insertar:

| (dejar en blanco) | Cambio realizado | Página (índice) |
|:---:|:---|:---:|
| | **Renombrado del fenómeno central:** «polarización editorial» → «polarización de la percepción mediática digital» (50+ apariciones, ES y EN). Los índices ICMC e IDN no cambian de nombre. | Todo el documento |
| | Cierre de la **Realidad Problemática (1.1)**: se enuncia el problema específico — un mismo candidato recibe trato opuesto según la fuente/plataforma que consume el electorado. | 9 |
| | **Problema General y PE1–PE5** reformulados (1.2); **Objetivo General y OE1–OE5** reformulados (1.3). | 10 |
| | **Hipótesis General y HE1–HE5** reformuladas; **HE3** con umbrales verificables **ICMC > 25** e **IDN > 50** (escala 0–200) (1.4). | 11 |
| | **Justificación teórica (1.5.1)**: aporte = operacionalizar la polarización mediante ICMC/IDN sobre grafos. | 11–12 |
| | **Justificación práctica (1.5.2)**: público objetivo reorientado a **entidades** (JNE/ONPE, verificadores/*fact-checking*, observadores, medios, analistas, consultoras); se retira a la **ciudadanía** como destinatario. | 12 |
| | **Justificación metodológica (1.5.3)**: cálculo de ICMC/IDN sobre las vistas Gold por fuente (`gold_sentimiento_fuente_diario`). | 13 |
| | **Delimitación conceptual (1.6.3)**: alcance (ICMC/IDN) y **límite explícito** — mide el *contenido* mediático, no la recepción/efecto en la audiencia ni la intención editorial. | 14 |
| | **Definición de polarización editorial vs. afectiva** + argumento de **bimodalidad** (no mera dispersión) (2.3.15). | 71 |
| | **Fórmulas de ICMC e IDN** añadidas (+ formalización de IRT/IVE); reencuadre de **cuatro a cinco indicadores** (2.3.17). | 73 |
| | **Matriz de operacionalización (Tabla 5)**: variable dependiente = polarización; indicadores **ISN por fuente, ICMC, IDN**. | 91 |
| | **Cómputo de ICMC e IDN** descrito en las técnicas de procesamiento (4.3). | 117 |
| | Nueva subsección **«Contraste y Validación de HE3»**; **Tabla de ICMC por candidato** (5 superan el umbral). | 149 |
| | **Tabla de ISN desagregado por fuente**; **Tabla de IDN** + análisis del **patrón bimodal** (prensa vs. TikTok). | 150 |
| | **Columna ICMC** añadida a la Tabla 31 de candidatos; **Dictamen de validación de HE3** (se acepta con datos reales). | 149–150 |
| | **Conclusiones** reescritas: OG, OE3, **OE5 (nueva)** y verificación de HG/HE3 con los nuevos umbrales (6.1). | 154–156 |
| | **Recomendación** sobre umbrales de polarización (ICMC/IDN); IVP/ICG como trabajo futuro (6.2). | 159 |
| | **Anexos regenerados**: árbol de problemas (166), árbol de objetivos (167), matriz de consistencia (168). | 166–168 |

Al terminar, devuelve la tabla completa con la **primera columna vacía** y las otras dos llenas.
