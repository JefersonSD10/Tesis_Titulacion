# Evaluación de dedup (consolidate + llm_dedup)

## Precision (merges del sistema confirmados como mismo_tema)

- n anotados: **2** (correctos 1 / incorrectos 1)
- precision = **0.500**
- intervalo inferior Wilson 95%: 0.095
- ⚠️ **muestra baja** (n=2). Cada caso pesa 50pp. Reportar como evidencia preliminar.

## Recall (falsos negativos detectados en sospechosos)

- n sospechosos anotados: **13** (FN 1 / TN 12)
- recall = **0.500**
- intervalo inferior Wilson 95%: 0.095

## F1 combinada

- F1 = **0.500**

## Falsos negativos del sistema (recall errors)

| par_id | tema_a | tema_b | cosine | actor_overlap | justificación |
|---|---|---|---|---|---|
| susp_6503_6577 | voto_confianza_gabinete_miralles | voto_confianza_gobierno | 0.831 | 0.333 | Ambos refieren al voto de confianza del gabinete Miralles. 'voto_confianza_gobierno' es una forma má |

## Merges incorrectos del sistema (precision errors)

| par_id | tema_a | tema_b | origen | justificación |
|---|---|---|---|---|
| merge_consolidate_6529_6555 | debates_presidenciales_perez_tello | entrevista_preguntas_fuego_perez_tello | consolidate | Comparten protagonista (Pérez Tello, actores=1.0) pero refieren a apariciones mediáticas distintas:  |