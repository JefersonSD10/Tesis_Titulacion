# Reportes de evaluación — baseline

Snapshot canónico de las métricas de calidad del sistema sobre el gold
dataset (100 docs + 15 pares de temas).

## Archivos

| Archivo | Contenido |
|---|---|
| `summary_baseline_v3.md` | Resumen ejecutivo de las dos evaluaciones (label + dedup). |
| `label_baseline_v3.md` | Métricas detalladas por dimensión (categoría, es_relevante_peru, candidato_protagonista, actores_principales, sentimiento_tema, tema_emergente) + matriz por clase + top errores. |
| `label_baseline_v3.json` | Misma data estructurada. |
| `dedup_baseline_v3.md` | Precision / recall / F1 de la agrupación de temas (consolidate + llm_dedup) + falsos negativos + merges incorrectos. |
| `dedup_baseline_v3.json` | Misma data estructurada. |

## Cómo se generaron

```bash
uv run python scripts/evaluate.py
```

El script lee:
- `tests/golden/dataset.yaml` — 100 docs anotados.
- `tests/golden/theme_pairs.yaml` — 15 pares de temas anotados.

Y consulta la BD Postgres en busca del `tema_emergente_real` que el LLM
le asignó a cada doc (vía `scripts/enrich_dataset_with_tema.py`).

## Versión de los reportes (v3 = 4 mejoras aplicadas)

Configuración del pipeline al momento de generar estos reportes:

- **Prompt v3**: regla #5 (`otros_no_politico`) ampliada para no
  capturar política con actores no-candidatos (infraestructura, servicios
  públicos, etc.). Bajó el bleed de 21% a 4%.
- **Schema**: campo `tema_fase` (emergente/desarrollo/cierre/aniversario).
- **Tiers**: 5/5/3/2/3 = 18 total. Tier 5 dormido (event) reemplazado
  por intraday fresh.
- **Grafo**: nodo `:Medio` + `[:PUBLICADO_POR]`. Propiedad `tema_fase`
  en `[:SOBRE_TEMA]`.

## Resultados clave

```
LABEL (n=100):
  categoria       accuracy=0.830  F1_macro=0.856
  es_relevante_peru accuracy=0.960  F1=0.974
  protagonista    accuracy=0.970
  actores_principales F1_micro=0.295  F1_macro=0.444
  sentimiento_tema MAE=0.107  r=0.892
  tema_emergente  exact=0.380  lenient=0.410  semantic=0.700

DEDUP (n=15: 2 mergeados + 13 sospechosos):
  precision=0.500  recall=0.500  F1=0.500  (muestra insuficiente)
```

## Re-generación

Estos reportes se versionan como **snapshot baseline**. Si se corre
`evaluate.py` localmente, se crean archivos nuevos con timestamp en este
mismo directorio — **no commitearlos** salvo que se quiera dejar otro
snapshot oficial (renombrar entonces a `*_baseline_vN.{md,json}` y
actualizar este README).

## Limitaciones documentadas

Detalladas en `docs/pendientes-evaluacion.md`. Resumen:

- Anotación por 1 anotador (sin Cohen κ inter-anotador).
- n=100 docs (el plan riguroso pide 500).
- Dedup precision con muestra pequeña (n=2 mergeados → cada caso pesa 50pp).
- Sin baselines comparativos (XLM-RoBERTa, SVM, GPT-4o).
- Sin coherencia $C_v$ (Röder 2015).
- Tema `actores_principales` con F1 micro 0.30: el LLM extrae ~1/3 de
  los actores principales que un humano marca como tales. Punto de
  mejora claro para iteración futura.
