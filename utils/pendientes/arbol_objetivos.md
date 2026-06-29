# Prompt de imagen — Anexo: Árbol de Objetivos

> Pega el siguiente prompt en un generador de imágenes (Gemini / DALL·E / Midjourney / etc.)
> para producir `anexos/arbol_objetivos.png`. Es el espejo en positivo del árbol de problemas.

**Prompt:**

Crea un diagrama tipo «árbol de objetivos» académico, como infografía limpia y profesional, en orientación horizontal (A4 apaisado), fondo blanco, tipografía sans-serif legible y texto en español sin errores ortográficos. Es el espejo en positivo de un árbol de problemas. Organízalo en tres niveles conectados por flechas verticales que apuntan hacia ARRIBA (los medios logran el objetivo, el objetivo conduce a los fines):

NIVEL INFERIOR — banda «MEDIOS» (cinco cajas en fila, tono verde-azulado):
- M1 (OE1): Integrar y armonizar contenido multi-fuente con arquitectura Medallion para hacerlo comparable entre fuentes.
- M2 (OE2): Analizar el discurso con LLMs asignando sentimiento dirigido y tópicos de forma consistente entre plataformas — ISN por fuente, IRT, IVE.
- M3 (OE3): Construir un grafo de conocimiento electoral que cuantifique la polarización de la percepción mediática digital mediante ICMC (magnitud) e IDN (estructura).
- M4 (OE4): Exponer el análisis vía GraphRAG sobre MCP para consulta en lenguaje natural, incluida la polarización detectada.
- M5 (OE5): Evaluar el sistema contra ground truth humano y LLM-as-judge, validando que la polarización detectada sea real.

NIVEL CENTRAL — banda «OBJETIVO CENTRAL» (una sola caja, grande y destacada, tono verde intenso, el elemento más prominente del diagrama):
«Detectar y cuantificar la polarización de la percepción mediática digital en el tratamiento de los candidatos presidenciales entre las distintas fuentes y plataformas digitales, mediante un sistema de inteligencia electoral basado en LLMs y grafos de conocimiento (caso Elecciones Presidenciales Perú 2026).»

NIVEL SUPERIOR — banda «FINES» (cuatro cajas en fila, tono celeste):
- F1: El electorado y los analistas pueden contrastar cómo difiere el tratamiento de un candidato según la fuente.
- F2: Verificadores y el JNE disponen de evidencia cuantificada (ICMC/IDN) para monitorear el equilibrio de la cobertura.
- F3: Menor exposición a las burbujas narrativas y a la desinformación.
- F4: Detección temprana de narrativas contrapuestas emergentes entre fuentes.

Estilo: cajas con esquinas redondeadas, líneas finas, etiqueta de banda («MEDIOS», «OBJETIVO CENTRAL», «FINES») a la izquierda de cada nivel, paleta sobria y composición equilibrada, apropiada para un anexo de tesis. Mantén la simetría estructural con el árbol de problemas.

---

> **Sugerencia:** los generadores de imagen raster suelen deformar texto largo. Para máxima fidelidad del texto, pide al modelo que lo entregue como **diagrama vectorial (SVG)** o como código (Graphviz/Mermaid) y expórtalo a PNG.
