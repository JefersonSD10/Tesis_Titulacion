# Prompt de imagen — Anexo: Árbol de Problemas

> Pega el siguiente prompt en un generador de imágenes (Gemini / DALL·E / Midjourney / etc.)
> para producir `anexos/arbol_problemas.png`.

**Prompt:**

Crea un diagrama tipo «árbol de problemas» académico, como infografía limpia y profesional, en orientación horizontal (A4 apaisado), fondo blanco, tipografía sans-serif legible y texto en español sin errores ortográficos. Organízalo en tres niveles conectados por flechas verticales que apuntan hacia ARRIBA (las causas generan el problema, el problema genera los efectos):

NIVEL INFERIOR — banda «CAUSAS» (cinco cajas en fila, tono azul-gris):
- C1: Enfoques tradicionales centrados en encuestas o en una sola fuente; no comparan el tratamiento entre plataformas.
- C2: Fuentes heterogéneas (prensa digital, X/Twitter, YouTube, TikTok) difíciles de hacer comparables entre sí.
- C3: El NLP clásico no capta el contexto político (ironía, jerga regional, sentimiento dirigido) de forma consistente entre fuentes.
- C4: Falta una representación estructurada (grafo) que cuantifique la divergencia de trato en una sola consulta.
- C5: No existen indicadores formales de la magnitud (ICMC) ni de la estructura par a par (IDN) de esa divergencia.

NIVEL CENTRAL — banda «PROBLEMA CENTRAL» (una sola caja, grande y destacada, tono rojo-naranja, el elemento más prominente del diagrama):
«La polarización de la percepción mediática digital —un mismo candidato presidencial recibe un tratamiento sistemáticamente opuesto según la fuente o plataforma que consume el electorado peruano— no se detecta ni se cuantifica de forma sistemática con los enfoques tradicionales.»

NIVEL SUPERIOR — banda «EFECTOS» (cuatro cajas en fila, tono ámbar):
- E1: El electorado que se informa por una sola plataforma recibe una imagen sesgada del candidato (burbujas narrativas).
- E2: Verificadores y el JNE carecen de evidencia cuantificada para monitorear el equilibrio de la cobertura.
- E3: Mayor susceptibilidad a la desinformación y a la volatilidad del comportamiento electoral.
- E4: No se detectan a tiempo las narrativas contrapuestas emergentes entre fuentes durante la campaña.

Estilo: cajas con esquinas redondeadas, líneas finas, etiqueta de banda («CAUSAS», «PROBLEMA CENTRAL», «EFECTOS») a la izquierda de cada nivel, paleta sobria y composición equilibrada, apropiada para un anexo de tesis.

---

> **Sugerencia:** los generadores de imagen raster suelen deformar texto largo. Para máxima fidelidad del texto, pide al modelo que lo entregue como **diagrama vectorial (SVG)** o como código (Graphviz/Mermaid) y expórtalo a PNG.
