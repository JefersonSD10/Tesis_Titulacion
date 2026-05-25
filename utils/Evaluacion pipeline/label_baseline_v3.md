# Evaluación de label (gold)

- Docs anotados: **100**
- Correcciones humanas al LLM: **2** (2%)

## Resumen

| Dimensión | Métrica | Valor |
|---|---|---|
| categoría | accuracy | **0.830** (83/100) |
| categoría | F1 macro | 0.856 |
| es_relevante_peru | accuracy | **0.960** |
| es_relevante_peru | F1 | 0.974 (P 1.000 / R 0.950) |
| candidato_protagonista | accuracy | 0.970 |
| candidato_protagonista | F1 macro | 0.916 |
| actores_principales | F1 micro | **0.295** (P 0.218 / R 0.457) |
| actores_principales | F1 macro | 0.444 |
| sentimiento_tema | MAE | **0.107** (n=100) |
| sentimiento_tema | RMSE | 0.160 |
| sentimiento_tema | Pearson r | 0.892 |
| sentimiento_tema | bias (pred - gold) | +0.051 |
| tema_emergente | accuracy exact | 0.380 (38/100) |
| tema_emergente | accuracy lenient (substring) | 0.410 (41/100) |
| tema_emergente | accuracy semantic (token overlap) | **0.700** (70/100) |

## Categoría — F1 por clase (clases con support > 0)

| categoría | precision | recall | F1 | support |
|---|---|---|---|---|
| economia | 0.917 | 0.846 | 0.880 | 13 |
| institucional | 0.700 | 0.778 | 0.737 | 9 |
| justicia_legalidad | 0.889 | 0.889 | 0.889 | 9 |
| corrupcion | 1.000 | 0.625 | 0.769 | 8 |
| educacion | 1.000 | 1.000 | 1.000 | 7 |
| salud | 1.000 | 1.000 | 1.000 | 7 |
| seguridad | 1.000 | 1.000 | 1.000 | 7 |
| social_genero | 1.000 | 0.571 | 0.727 | 7 |
| internacional | 1.000 | 0.833 | 0.909 | 6 |
| infraestructura | 1.000 | 0.800 | 0.889 | 5 |
| otros_no_politico | 0.286 | 0.800 | 0.421 | 5 |
| regional | 1.000 | 0.400 | 0.571 | 5 |
| deportes | 1.000 | 1.000 | 1.000 | 4 |
| entretenimiento | 0.600 | 1.000 | 0.750 | 3 |
| ambiental | 1.000 | 1.000 | 1.000 | 2 |
| farandula | 1.000 | 1.000 | 1.000 | 2 |
| humor_satira | 1.000 | 1.000 | 1.000 | 1 |

## Errores de asignación doc → tema (top 20)

| doc_id | gold | pred (LLM) | título |
|---|---|---|---|
| 28184 | desastre_natural_vraem | desborde_rio_sankiruato_ayna | Desborde del río Sankiruato deja casas inhabitables en Ayna \| Primera Edición \| Noticias |
| 29086 | crisis_combustibles_transporte | crisis_gas_glp_chiclayo | 🚨La incertidumbre por el suministro de gas también afecta al transpor... |
| 29131 | crisis_energetica_lima | reparacion_ductos_gas_camisea | 🔴 La explosión del ducto de gas de Camisea volvió a poner en evidenci... |
| 28275 | clases_virtuales_polemica | crisis_combustibles_peru | 🏫Padres de familia en desacuerdo con las clases virtuales por crisis ... |
| 29058 | controversia_grozo_espionaje | grozo_espia_ecuador | #Enfrentados El candidato de Integridad, Wolfgang Grozo, no quiso acl... |
| 28260 | clases_virtuales_polemica | crisis_energetica_lima | 🚨El presidente José Balcázar, durante la conferencia de prensa del Co... |
| 29117 | propuestas_seguridad_candidatos | plan_seguridad_luna_urresti | #FuegoCruzadoElectoral \| ¡La dupla que busca cambiar la seguridad del... |
| 29030 | caso_hurtado_trafico_influencias | denuncia_papeletas_irregulares_magdalena | 🚨 El Ministerio Público solicitó al Poder Judicial extender la medida... |
| 28141 | gestion_municipal_piura | inasistencias_regidores_piura | Piura: Los regidores de minoría frenan la titulación y los convenios sociales |
| 27955 | corte_agua_sedapal_lima | cierre_puente_variante_uchumayo | Sedapal anuncia corte agua hasta por 10 horas en un distrito de Lima este martes 10 de mar |
| 25853 | tramite_dni_reniec | colas_reniec_santa_anita | Colas desde la madrugada en Reniec: usuarios señalan retrasos y expresan malestar |
| 27747 | operativos_pnp_lima | operativo_extorsion_lima_norte | Intervienen a más de 30 personas en una fiesta en San Martín de Porres: PNP halló un arma  |
| 28169 | cierre_puente_uchumayo_arequipa | colapso_puente_sahuay_ancash | Habilitan vía alterna para ingreso y salida de Arequipa tras cierre del puente Uchumayo |
| 27881 | crisis_combustibles_transporte | incremento_pasajes_lima | Pasajeros expresan malestar por incremento del precio de pasajes: “Hay abuso” |
| 28164 | crisis_combustibles_transporte | paro_buses_sit_arequipa | Pasaje en Arequipa se mantendrá en S/1 y descartan el tránsito libre |
| 28095 | crisis_combustibles_transporte | paro_buses_sit_arequipa | Municipalidad de Arequipa evalúa declarar “tránsito libre” ante la falta de buses del SIT |
| 27848 | crisis_energetica_lima | crisis_combustibles_peru | “Hay abuso”: buses, colectivos y hasta mototaxis suben precios de pasajes por crisis energ |
| 25580 | crisis_venezuela_estados_unidos | tension_iran_eeuu | María Corina Machado no da fecha de su vuelta a Venezuela y dice que Donald Trump es un "a |
| 27834 | corrupcion_candidatos_fuerza_popular | denuncia_papeletas_irregulares_magdalena | Ucayali: candidata a diputada por Fuerza Popular fue declarada reo contumaz |
| 25612 | caso_cerron_indulto | habeas_corpus_cerron_tc | Héctor Acuña sobre Vladimir Cerrón: "Imagínese premiar a una persona que está prófugo y se |
| 28151 | retraso_puente_arequipa_la_joya | colapso_puente_sahuay_ancash | No reinician trabajos en Puente Arequipa - La Joya por falta de Seguro CAR |
| 28999 | caso_villar_marzano | denuncia_papeletas_irregulares_magdalena | Adrián Villar habría viajado Cajamarca tras causar la muerte a Lizeth Marzano |
| 28135 | conflicto_tierras_catacaos | denuncia_papeletas_irregulares_magdalena | Comuneros exigen inspección fiscal en terreno comunal en Piura |
| 27894 | nuevo_gobierno_kast_chile | tension_iran_eeuu | “El mayor reto para Kast será mostrar resultados rápidos porque ofreció un giro radical en |
| 28092 | clases_virtuales_polemica | crisis_combustibles_peru | Gerencia de Educación de Arequipa dispone clases virtuales para los colegios privados |

## Errores de categoría (top 20)

| doc_id | gold | pred | título | nota |
|---|---|---|---|---|
| 25853 | institucional | otros_no_politico | Colas desde la madrugada en Reniec: usuarios señalan retrasos y expresan malesta |  |
| 25930 | corrupcion | justicia_legalidad | Chiclayo: exsubgerente de alcalde de JLO es condenada por cobrar por puesto en p |  |
| 26054 | economia | institucional | ComexPerú: “Los empleados deben pasar por la medición de resultados” |  |
| 26057 | corrupcion | otros_no_politico | Chiclayo: exsubgerente de alcalde de JLO es condenada por cobrar por puesto en p |  |
| 27774 | economia | otros_no_politico | Pueblo Libre y Barranco impulsan el alza de alquileres en Lima mientras mejora l |  |
| 27778 | otros_no_politico | entretenimiento | "Multiplica la ausencia del amigo": Joaquín Sabina dedica dos poemas a Alfredo B |  |
| 27955 | infraestructura | otros_no_politico | Sedapal anuncia corte agua hasta por 10 horas en un distrito de Lima este martes |  |
| 28054 | corrupcion | otros_no_politico | Huánuco: compra de equipos biométricos para hospital incumplió normas técnicas y |  |
| 28073 | social_genero | institucional | MOVIMIENTO EN LAS ENCUESTAS, columna de Iván Slocovich Pardo |  |
| 28095 | regional | economia | Municipalidad de Arequipa evalúa declarar “tránsito libre” ante la falta de buse |  |
| 28141 | regional | otros_no_politico | Piura: Los regidores de minoría frenan la titulación y los convenios sociales |  |
| 28176 | social_genero | entretenimiento | Este es el personaje político que lidera las encuestas de Datum | Encuesta Datum → social_genero (regla #4). |
| 28195 | internacional | otros_no_politico | Trump no descarta invasión terrestre en Irán \| El Comercio | Internacional sin Perú (regla #3). |
| 28262 | institucional | otros_no_politico | 🚨"César Acuña está en la cima", así lo afirmó el candidato presidenci... |  |
| 28282 | regional | otros_no_politico | 🗣 Vecinos de Magdalena denuncian proceso irregular de papaeletas #mag... |  |
| 28999 | justicia_legalidad | otros_no_politico | Adrián Villar habría viajado Cajamarca tras causar la muerte a Lizeth Marzano |  |
| 29103 | social_genero | institucional | 🗳️📊César Acuña aumentó intención de voto entre jóvenes de 18 y 24 año... |  |