# Árbol de Objetivos — versión polarización editorial

> Reemplaza la imagen `anexos/arbol_objetivos.png`. Es el espejo en positivo del
> árbol de problemas: medios → objetivo central → fines.

## Objetivo central
**Detectar y cuantificar la polarización editorial** en el tratamiento de los
candidatos presidenciales entre las distintas fuentes y plataformas digitales,
mediante un sistema de inteligencia electoral basado en LLMs y grafos de
conocimiento (caso Elecciones Presidenciales Perú 2026).

## Medios (van debajo del objetivo; espejo de las causas)
- **M1.** Integrar y armonizar contenido multi-fuente (Medallion) para hacerlo
  **comparable entre fuentes**. (OE1)
- **M2.** Analizar el discurso con LLMs asignando sentimiento dirigido y tópicos
  de forma consistente entre plataformas — **ISN** por fuente, **IRT**, **IVE**.
  (OE2)
- **M3.** Construir un grafo de conocimiento electoral que **cuantifique la
  polarización editorial** mediante **ICMC** (magnitud) e **IDN** (estructura).
  (OE3)
- **M4.** Exponer el análisis vía GraphRAG sobre MCP para consulta en lenguaje
  natural, incluida la polarización editorial. (OE4)
- **M5.** Evaluar el sistema contra *ground truth* humano y LLM-as-judge,
  validando que la polarización detectada sea real. (OE5)

## Fines (van arriba del objetivo; espejo de los efectos)
- **F1.** El electorado y los analistas pueden contrastar cómo difiere el
  tratamiento de un candidato según la fuente.
- **F2.** Verificadores, JNE y ciudadanía disponen de evidencia cuantificada
  (ICMC/IDN) para monitorear el equilibrio de la cobertura.
- **F3.** Menor exposición a burbujas narrativas y a la desinformación.
- **F4.** Detección temprana de narrativas contrapuestas emergentes entre fuentes.

```
        ┌─────────────────────── FINES ─────────────────────────┐
        F1 contraste   F2 evidencia      F3 menor          F4 alerta
        entre fuentes  ICMC/IDP p/       desinformación    temprana
                       verificar
                              ▲
        ┌─────────────── OBJETIVO CENTRAL ───────────────┐
        │  Detectar y cuantificar la polarización          │
        │  editorial entre fuentes/plataformas mediante     │
        │  LLMs + grafos de conocimiento                    │
        └──────────────────────────────────────────────────┘
                              ▲
        ┌─────────────────────── MEDIOS ────────────────────────┐
        M1 ingesta   M2 análisis   M3 grafo +    M4 GraphRAG  M5 evaluación
        Medallion    LLM (ISN/     ICMC/IDN      sobre MCP    vs ground
        comparable   IRT/IVE)                                  truth
```
