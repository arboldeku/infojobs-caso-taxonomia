# Hallazgo: homonimia de "instrumentista" en la clasificación ESCO "músico/música" — 14/09/2026

**Fecha:** 14/09/2026
**Repo:** `infojobs-caso-taxonomia` (commit `56cd60d` en `main`)
**Estado:** análisis completado, notebook ejecutado sin errores, commiteado y pusheado a un repo privado. Entrega prevista: miércoles 16/09/2026. Entrevista final (Anna Via, Head of AI, Infojobs): jueves 17/09/2026, 12:00 CET.
**Documentos previos que hay que conocer:** `README.md` de este repo, `docs/propuesta_prueba_tecnica.pdf` (enunciado original del caso), `notebooks/analisis_taxonomia_musico.ipynb` (análisis ejecutado con outputs reales).

---

## 0. Para quien llegue aquí sin contexto (en llano)

InfoJobs clasifica ofertas de empleo y experiencias de candidatos usando la taxonomía pública **ESCO** (el estándar europeo de ocupaciones). Nos dieron un CSV con 210 títulos de puesto distintos que un modelo predictivo había clasificado como la posición ESCO "músico/música", cada uno con un score de confianza y cuántas filas (ofertas + experiencias) tenía ese título.

La primera intuición al abrir el fichero es que hay un problema evidente: hay títulos como "instrumentista maestro planta" o "instrumentista quirófano" que claramente no son músicos, mezclados con "guitarrista" o "saxofonista" que sí lo son. La pregunta fácil sería "el modelo se equivoca, hay que arreglarlo". Pero investigando un paso más allá, contra la propia API oficial de ESCO, la conclusión cambia: **"instrumentista" es un sinónimo oficial de ESCO para "músico/música"**. El modelo no se lo inventó — lo hereda del diccionario que le dieron. El problema real es que esa palabra es un homónimo genuino en el mercado laboral español (músico, técnico de instrumentación industrial, instrumentista de quirófano son tres oficios sin relación entre sí que comparten la misma etiqueta), y ni ESCO ni el modelo tienen una forma de desambiguar cuál es cuál según el contexto del título completo.

Cuantificado: de las 879 filas totales asignadas a "músico/música", el **47% no son música** (instrumentación industrial o instrumentista quirúrgico), y ese volumen tiene un score medio de 0.75 — no es ruido de baja confianza, el modelo está "seguro" de una parte sustancial de sus errores. Un solo título, "instrumentista" sin ninguna palabra más, representa el **19.3% de todo el volumen** por sí solo.

---

## 1. El problema tal como llegó

1. **La tarea:** validar la calidad de la clasificación a "músico/música" a partir de un CSV agregado por título original, respondiendo tres preguntas (calidad, problema/motivo, sinónimos que falten) y proponiendo una automatización IA para una tarea manual identificada.
2. **La primera lectura superficial del CSV** sugiere un modelo con ruido aleatorio: hay títulos claramente ajenos a la música mezclados con títulos musicales legítimos.
3. **La hipótesis inicial descartada:** se exploró primero (vía WebFetch, sin verificar con la fuente cruda) si el sinónimo oficial de ESCO era "instrumentalista" y no "instrumentista" — lo que habría convertido esto en un simple error de fuzzy-matching del modelo. Esa hipótesis resultó ser una alucinación del resumen automático de la primera consulta, no un dato real. Se corrigió consultando la API de ESCO directamente por `curl` y parseando el JSON crudo (ver sección 2) antes de dar la conclusión por buena — **la lección concreta: no fiarse de un resumen de IA sobre una fuente externa sin verificar contra el dato crudo cuando la conclusión es central para el caso.**
4. **La presión real:** dos días de margen antes de la entrega (miércoles 16/09) en el momento de este informe, y esta es la pieza central del análisis que se presenta a la Head of AI en la entrevista final — no hay margen para llevar una cifra o una causa raíz equivocada a esa sala.

---

## 2. Qué se investigó antes de construir nada (procedimiento `/historic`)

- **`README.md` y el enunciado original** (`docs/propuesta_prueba_tecnica.pdf`): confirmaron que el entregable pedía explícitamente cruzar el CSV con la definición ESCO de la posición, no solo analizar el fichero de forma aislada.
- **Verificación directa contra la API pública de ESCO**, en dos pasos porque el primero resultó no fiable:

  | Intento | Método | Resultado |
  |---|---|---|
  | 1 | WebFetch con resumen automático | Devolvió una lista de sinónimos que incluía "instrumentalista" — **no verificado, resultó ser una alucinación parcial** |
  | 2 | `curl` directo al endpoint `resource/occupation` + parseo del JSON crudo en Python | Lista real y reproducible de 14 sinónimos oficiales en español, **"instrumentista" incluido** |

- **Cuantificación con los datos reales del CSV** (ejecutado en el notebook, no estimado):

  | Fuente | Dato | Qué significa |
  |---|---|---|
  | CSV completo | 209 títulos distintos, 879 filas totales | Universo del análisis |
  | Coincidencia exacta con sinónimo oficial ESCO | 402 de 879 filas (45.7%) | Menos de la mitad del volumen es una coincidencia limpia con la lista oficial |
  | Título "instrumentista" (solo, sin contexto) | 170 filas, score 1.00 | Un único título ambiguo por definición representa el 19.3% de todo el volumen |
  | Clusters no-musicales (industrial + quirúrgico + sin contexto claro) | 413 filas (47.0%), score medio ponderado 0.751 | Casi la mitad del volumen no es música, y el modelo tiene confianza alta en ese error |
  | Cluster "música real" (términos inequívocos: guitarrista, saxofonista, etc.) | 469 filas, score medio ponderado 0.930 | La clasificación es fiable cuando el sinónimo no es ambiguo |

Conclusión de esta fase: **el diccionario de ESCO es la causa raíz, no el modelo por sí solo.** Esto cambia completamente qué se recomienda arreglar (ver sección 4).

---

## 3. Qué se construyó (parte técnica)

Todo en `infojobs-caso-taxonomia`, notebook `notebooks/analisis_taxonomia_musico.ipynb`, ejecutado de principio a fin sin errores (7 celdas de código, 0 celdas con error).

### 3.1 Carga y limpieza (`pandas`)

El CSV original venía en UTF-16 con tabulador como separador y decimales en formato español (coma). Normalizado a un DataFrame estándar con `score_medio` y `num_rows` como float.

### 3.2 Contraste contra ESCO (`requests` + API pública)

Llamada a `https://ec.europa.eu/esco/api/resource/occupation` con la URI del concepto "músico/música" (`f6803a58-91d9-4251-9a07-09fc387cec16`), parseando `alternativeLabel.es` del JSON de respuesta. Reproducible por cualquiera que ejecute la celda — no depende de una captura de pantalla ni de un resumen.

### 3.3 Clasificación heurística en 4 clusters (keyword-matching)

`musica_real` / `instrumentacion_industrial` / `instrumentista_quirurgico` / `sin_contexto_claro`, usando listas de palabras clave por dominio (ver notebook, sección 4). Es una primera pasada, no un modelo entrenado — suficiente para cuantificar el tamaño del problema, no para producción.

### 3.4 Ejemplos concretos de falsos positivos de alta confianza

Tabla filtrada de títulos no-musicales con score ≥ 0.8, ordenada por volumen — la evidencia que sostiene la afirmación "el modelo está seguro de una parte sustancial de sus errores".

---

## 4. Por qué NO se recomienda añadir sinónimos nuevos (la pregunta 3 del caso tiene trampa)

El enunciado pregunta explícitamente "¿se podrían sugerir nuevos sinónimos muy usados en títulos que actualmente no se tienen en cuenta?". La respuesta honesta, después de este análisis, es que **esa no es la palanca correcta**:

1. La cobertura de sinónimos musicales genuinos ya es buena — guitarrista, saxofonista, percusionista, clarinetista, flautista, trompetista, etc. tienen score ~1.00 y están ya en la lista oficial de ESCO.
2. El problema dominante es el contrario: **sobra** cobertura del término ambiguo "instrumentista", no falta cobertura de términos musicales.
3. Añadir sinónimos nuevos sin resolver la ambigüedad de "instrumentista" no reduciría el 47% de volumen mal clasificado — sería trabajo en la dirección equivocada.

Lo que sí se recomienda: una capa de exclusión/desambiguación contextual para los sinónimos ya identificados como ambiguos (ver la propuesta de automatización IA, sección 7 del notebook), no una expansión de la lista de sinónimos.

---

## 5. Resultado final

- **Lo que responde el análisis:** las tres preguntas del caso, con cifras extraídas directamente de la ejecución del notebook (no estimadas de memoria), más una propuesta de automatización IA acotada al subconjunto de sinónimos ambiguos en vez de a todo el volumen.
- **Lo que NO se ha hecho:** entrenar o modificar el modelo de clasificación real de InfoJobs (fuera de alcance del caso — el caso pide análisis y propuesta, no implementación en producción). Tampoco se ha verificado si "electricista instrumentista"/"electro instrumentista" (con errores tipográficos en el dataset original) deberían reclasificarse — quedan en el cluster `sin_contexto_claro` porque el keyword-matching actual no captura variantes con typos; mencionar esto como limitación conocida si se pregunta.
- **Pruebas realizadas:** notebook ejecutado de principio a fin con `nbconvert --execute`, 0 celdas con error, verificación manual de que el JSON de salida no tiene corrupción de encoding (el `músico` con tilde se mostraba mal en la terminal de Windows por un problema de codificación de la consola, no del dato — confirmado escribiendo el output a un fichero UTF-8 y releyéndolo).

### Caveats para la entrevista

1. La clasificación en 4 clusters es una heurística de keyword-matching manual para este análisis exploratorio, no un modelo entrenado — si Anna pregunta por la robustez del método, la respuesta honesta es que es un primer corte cuantitativo, no una solución de producción lista para desplegar.
2. La cifra del 47% depende de las listas de palabras clave elegidas — es una aproximación razonable, no una cifra exacta e inmutable. Si se refina el keyword-matching (por ejemplo, capturando los typos "electicista"/"electro"), el número puede moverse ligeramente, probablemente al alza.
3. El hallazgo de que "instrumentista" es sinónimo oficial de ESCO es el dato más importante de todo el análisis y está verificado contra la API cruda (no un resumen) — este es el punto que sostiene la credibilidad de la conclusión completa. Ver sección 2 de este informe para el detalle de por qué se verificó dos veces.

---

## 6. Lección para el `README.md` de este repo (pendiente de añadir)

> El hallazgo central del caso es que "instrumentista" es un sinónimo **oficial** de ESCO para "músico/música" (verificado contra la API cruda, no un resumen), y que esa palabra es homónima real en el mercado laboral español con instrumentación industrial e instrumentista quirúrgico. El 47% del volumen clasificado como músico no es música, con score medio 0.75 (alta confianza incorrecta). La recomendación NO es añadir sinónimos musicales nuevos — la cobertura ya es buena — sino añadir una capa de desambiguación contextual para los sinónimos ya identificados como ambiguos.
