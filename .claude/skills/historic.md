---
name: historic
description: >
  Uso OBLIGATORIO antes de sacar cualquier conclusión nueva, cambiar un
  número ya calculado, o decidir cómo se presenta un hallazgo del caso
  práctico — sin excepción. Busca el historial real del análisis en
  `README.md` y, sobre todo, en `docs/` (los informes de sesión de este
  repo, al estilo "All Informs Prisma"). Ningún agente decide con criterio
  propio antes de agotar esta búsqueda — el historial ya verificado
  siempre tiene prioridad sobre una intuición nueva.
---

## Cuándo usarlo

SIEMPRE, antes de:
- Recalcular o reinterpretar un número que ya salió en el notebook o en un
  informe anterior (ej. el % de volumen no-musical, el peso de
  "instrumentista" solo, la lista de sinónimos ESCO).
- Añadir una conclusión nueva a la respuesta de las 3 preguntas del caso.
- Cambiar la propuesta de automatización IA.
- Asumir que algo "no se ha comprobado todavía" — probablemente ya se
  comprobó y está en `docs/`.

No es un "nice to have": si el candidato dice una cosa en la entrevista
con Anna y el notebook dice otra, o dice algo distinto a lo que ya le
contamos por chat, el coste no es técnico — es credibilidad en tiempo
real, delante de la persona que decide.

## Procedimiento

1. **Leer `README.md`** de este repo — contexto general del caso y
   estructura del proyecto.
2. **Buscar en `docs/`** con Grep, usando las palabras clave del tema
   (ESCO, instrumentista, cluster, score, sinónimo...). No asumir que un
   primer grep sin resultados significa que no hay nada — probar variantes
   antes de concluir que no existe historial.
3. **Leer los informes relevantes completos**, no solo el título — las
   cifras exactas y los matices ("por qué NO añadimos sinónimos nuevos",
   por ejemplo) suelen estar en el cuerpo del informe, no en el índice.
4. **Contrastar contra el propio notebook** (`notebooks/analisis_taxonomia_musico.ipynb`)
   ejecutado: los números de los outputs son la fuente de verdad, no una
   cifra recordada de memoria.
5. **Si el historial contradice lo que se iba a proponer, el historial
   gana** — no se aplica criterio nuevo por encima de un dato ya
   verificado (ej. contra la API de ESCO). Si de verdad se cree que el
   historial está equivocado o desactualizado, se señala explícitamente al
   usuario para que decida — nunca se ignora en silencio.
6. **Si no se encuentra nada relevante tras una búsqueda real**, decirlo
   explícitamente: "no encontré historial sobre X en README.md ni en
   docs/" — así el usuario sabe que se buscó y no que se saltó el paso.

## Por qué importa especialmente aquí

Este no es un repo de producción con un equipo detrás — es un caso técnico
de entrevista. El "incidente" que esta skill previene no es un bug en
código, es que el candidato llegue a la entrevista final defendiendo una
cifra o una conclusión que ya se cambió o se descartó en una sesión
anterior, sin darse cuenta. `docs/` existe para que eso no dependa de la
memoria de nadie.
