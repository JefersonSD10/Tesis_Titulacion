# Pendientes de modificación manual (imágenes y decisiones de asesor)

Estos artefactos **no se editan desde LaTeX** porque son imágenes PNG o
requieren decisión del asesor. Aquí están sus **versiones actualizadas en
texto/markdown** con la nueva formulación (polarización de la percepción mediática digital · ICMC/IDN)
para que regeneres los PNG o decidas.

| Archivo | Reemplaza a | Acción |
|---|---|---|
| `matriz_consistencia.md` | `anexos/matriz_consistencia.png` | Regenerar PNG con esta matriz |
| `arbol_problemas.md` | `anexos/arbol_problemas.png` | Regenerar PNG con este árbol |
| `arbol_objetivos.md` | `anexos/arbol_objetivos.png` | Regenerar PNG con este árbol |

## Decisiones que quedaron fuera de la edición automática

### Ítem 37 — Título / subtítulo (NO modificado)
El documento de correcciones recomienda **no cambiar el título sin indicación
del asesor** (efecto de arrastre alto: carátula, encabezados de cada página y
metadatos del PDF). Por eso el título se dejó **intacto**. Si el asesor pide
que el subtítulo mencione "polarización de la percepción mediática digital", los puntos a tocar son:
- `0/titulo.tex` (carátula)
- `tesis.tex` — `pdftitle`/`pdfsubject` (metadatos hyperref) y el encabezado
  `fancyhdr` de la página (`\fancyhead[L]{...}`).

### Ítem 38 — Antecedente sobre polarización mediática (OPCIONAL, no insertado)
Es opcional y requiere una **cita real verificable**; no se inventó ninguna.
Recomendación: añadir en `2/1_antecedentes/antecedentes.tex` un antecedente
sobre **divergencia/polarización de la percepción mediática digital entre plataformas** (p. ej. estudios
de *cross-platform sentiment divergence* o *media bias by outlet*). Si me pasas
un paper concreto (autor, año, DOI), lo redacto e inserto con el mismo formato
que los 7 antecedentes existentes.
