# Prompt de imagen — Anexo: Matriz de Consistencia

> Pega el siguiente prompt en un generador de imágenes (o pídelo como tabla/diagrama)
> para producir `anexos/matriz_consistencia.png`. El texto íntegro de cada celda está
> en la tesis (§1.2 Problemas, §1.3 Objetivos, §1.4 Hipótesis); aquí va condensado
> para que quepa legible en la imagen.

**Prompt:**

Crea una imagen de una **matriz de consistencia** de tesis: una tabla limpia y profesional, orientación horizontal (A4 apaisado), fondo blanco, encabezados con fondo de color y texto blanco, filas con líneas finas, tipografía sans-serif legible y texto en español sin errores ortográficos. Cinco columnas con estos encabezados: **Problema · Objetivo · Hipótesis · Variables · Indicadores / Metodología**. Una primera fila «GENERAL» destacada y cinco filas específicas numeradas (1 a 5); resalta la **fila 3** con un sombreado distinto porque carga el concepto central. Contenido condensado de cada celda:

**Fila GENERAL**
- Problema (PG): ¿Cómo diseñar un sistema de inteligencia electoral con LLMs y grafos de conocimiento que detecte y cuantifique la polarización de la percepción mediática digital —trato sistemáticamente opuesto de un mismo candidato entre fuentes/plataformas que consume el electorado— en las elecciones del Perú 2026, superando los enfoques por encuestas o fuentes aisladas?
- Objetivo (OG): Diseñar e implementar ese sistema que detecte y cuantifique la polarización de la percepción mediática digital en el tratamiento de los candidatos entre fuentes y plataformas.
- Hipótesis (HG): LLMs + grafos permiten detectar y cuantificar de forma verificable esa polarización, con calidad contrastable contra ground truth humano y LLM-as-judge.
- Variables: Indep. = LLMs + grafos de conocimiento. Dep. = polarización de la percepción mediática digital.
- Indicadores: ISN (insumo), ICMC (magnitud), IDN (estructura); calidad F1 macro, MAE. Enfoque CRISP-DM, corpus multi-fuente.

**Fila 1**
- PE1: ¿Arquitectura Medallion que integre prensa, X/Twitter, YouTube y TikTok preservando trazabilidad para que el contenido sea comparable entre fuentes?
- OE1: Diseñar e implementar esa arquitectura de ingesta y armonización multi-fuente, garantizando comparabilidad entre fuentes.
- HE1: Una arquitectura Medallion integra datos multi-fuente preservando consistencia y comparabilidad, reduciendo el costo del análisis LLM con filtros pre-modelo.
- Indicador: comparabilidad / trazabilidad del corpus; reducción de volumen pre-LLM.

**Fila 2**
- PE2: ¿Usar LLMs para el análisis contextual (sentimiento dirigido, ironía, jerga peruana, tópicos) con sentimiento por candidato consistente y comparable entre fuentes?
- OE2: Modelo de análisis semántico con LLMs que asigne sentimiento y tópicos consistentes, operacionalizado con ISN por fuente e índices IRT e IVE.
- HE2: LLMs few-shot ajustados al contexto peruano logran F1 macro ≥ 0.80 y MAE ≤ 0.15 en ISN y tópicos (IRT/IVE).
- Indicador: ISN por fuente, IRT, IVE; F1 macro ≥ 0.80, MAE ≤ 0.15.

**Fila 3 (clave — resaltar)**
- PE3: ¿Grafo de conocimiento que represente relaciones candidato-medio-tópico-tiempo y permita cuantificar la polarización de la percepción mediática digital (magnitud y estructura del trato opuesto entre fuentes)?
- OE3: Construir el grafo (extracción semántica con LLMs) que cuantifique esa polarización por candidato con ICMC (magnitud agregada) e IDN (pares con tratamiento opuesto).
- HE3: El grafo cuantifica en una única consulta la polarización, detectando polarización significativa (ICMC > 25) y pares con tratamiento opuesto (IDN > 50, escala 0–200).
- Indicador: ICMC (umbral > 25), IDN (umbral > 50).

**Fila 4**
- PE4: ¿GraphRAG sobre MCP para consultar en lenguaje natural el ecosistema electoral, incluida la polarización detectada, combinando recuperación textual y razonamiento estructural?
- OE4: Implementar el GraphRAG que combine recuperación semántica y razonamiento estructural para responder consultas, incluidas las de polarización entre fuentes.
- HE4: GraphRAG sobre MCP mejora relevancia y corrección de respuestas frente a un RAG por similitud vectorial sobre el mismo corpus.
- Indicador: Answer Correctness, relevancia, selección de herramientas.

**Fila 5**
- PE5: ¿Evaluar la calidad del sistema (precisión semántica, coherencia temática, fidelidad relacional, utilidad) validando que los patrones —incluida la polarización— sean reales y no artefactos del modelo?
- OE5: Evaluar el desempeño técnico y analítico contra ground truth humano y LLM-as-judge, validando que la polarización detectada sea real.
- HE5: El sistema alcanza calidad analítica y coherencia temática verificables, validando que la polarización detectada corresponda a fenómenos reales.
- Indicador: F1 macro, MAE, Answer Correctness; validación contra ground truth.

Asegura que cada fila quede alineada horizontalmente (Problema ↔ Objetivo ↔ Hipótesis de la misma fila tratan el mismo tema) y que el texto sea legible.

---

> **Sugerencia:** una tabla con tanto texto se renderiza mucho mejor con una herramienta de **tablas/diagramas** (o pidiendo el resultado como **HTML/SVG** y exportándolo a PNG) que con un generador de imagen raster, que tiende a deformar el texto largo.
