# Cambios para la observación del jurado — lista y página

**Observación respondida (Acta N.º 1402-2026-T):** *"Reformular problemas, objetivos e hipótesis procurando identificar un problema específico."*

**Commits que aplican estos cambios:** `d707020` (reformulación → ICMC/IDN), `327bd0c` (renombrado del fenómeno + público objetivo), `d20416c` (regeneración de los anexos PNG).

**Página = número impreso (el del índice).** Cuando varios cambios caen en la misma página, comparten fila.

| Pág. (índice) | Cambio(s) |
|:---:|:---|
| Todo el doc. | **Renombrado del fenómeno central:** «polarización editorial» → **«polarización de la percepción mediática digital»** (50+ apariciones, ES + EN). Los índices **ICMC** e **IDN** no cambian de nombre. |
| 9 | Cierre de la **Realidad Problemática (1.1)**: se enuncia el problema específico — un mismo candidato recibe trato opuesto según la fuente/plataforma. |
| 10 | • **Problema General y PE1–PE5** reformulados (1.2).<br>• **Objetivo General y OE1–OE5** reformulados (1.3). |
| 11 | **Hipótesis General y HE1–HE5** reformuladas; **HE3** con umbrales verificables **ICMC > 25** e **IDN > 50** (escala 0–200) (1.4). |
| 11-12 | **Justificación teórica (1.5.1)**: aporte = operacionalizar la polarización mediante ICMC/IDN sobre grafos. |
| 12 | **Justificación práctica (1.5.2)**: público objetivo reorientado a **entidades** (JNE/ONPE, verificadores/*fact-checking*, observadores, medios, analistas, consultoras); se retira a la **ciudadanía** como destinatario. |
| 13 | **Justificación metodológica (1.5.3)**: cálculo de ICMC/IDN sobre las vistas Gold por fuente (`gold_sentimiento_fuente_diario`). |
| 14 | **Delimitación conceptual (1.6.3)**: se añade el alcance (ICMC/IDN) y el **límite explícito** — el estudio mide el *contenido* mediático, no la recepción/efecto en la audiencia ni la intención editorial. |
| 71 | **Definición de polarización editorial vs. afectiva** + argumento de **bimodalidad** (no mera dispersión) (2.3.15). |
| 73 | • **Fórmulas de ICMC e IDN** añadidas (+ formalización de IRT/IVE) (2.3.17).<br>• Reencuadre: de **cuatro a cinco indicadores**. |
| 91 | **Matriz de operacionalización (Tabla 5)**: variable dependiente = polarización; dimensiones/indicadores = **ISN por fuente, ICMC, IDN**. |
| 117 | **Cómputo de ICMC e IDN** descrito en las técnicas de procesamiento (4.3). |
| 149 | • Nueva subsección **«Contraste y Validación de HE3»**.<br>• **Tabla de ICMC por candidato** (5 superan el umbral). |
| 150 | • **Tabla de ISN desagregado por fuente**.<br>• **Tabla de IDN** + análisis del **patrón bimodal** (prensa vs. TikTok). |
| 149-150 | • **Columna ICMC** añadida a la Tabla 31 de candidatos.<br>• **Dictamen de validación de HE3** (se acepta con datos reales). |
| 154-156  | **Conclusiones** reescritas: OG, OE3, **OE5 (nueva)** y verificación de HG/HE3 con los nuevos umbrales (6.1). |
| 159 | **Recomendación** sobre umbrales de polarización (ICMC/IDN); IVP/ICG quedan como trabajo futuro (6.2). |
| 166-168 | **Anexos regenerados**: árbol de problemas (166), árbol de objetivos (167), matriz de consistencia (168). |

---

> **Nota sobre las páginas.** Se extrajeron del compilado local del 26-jun (que sí contiene la reformulación). Pueden moverse **±1–2 páginas** en la compilación final limpia, porque: (a) ese compilado aún tiene referencias `??` sin resolver, y (b) no incluye el renombrado del término (`327bd0c`).
>
> ⚠️ El `tesis.pdf` versionado en el repo (commit `d20416c`) **está obsoleto y mal compilado** (no contiene la reformulación; 102 referencias `??`). Conviene **recompilar limpio** con Docker e idealmente recommitear el PDF correcto. Tras esa recompilación puedo regenerar esta tabla con las páginas exactas.
