---
name: supervise
description: >
  Uso cuando el usuario escribe "/supervise" al final de una tarea, o pide
  un resumen de lo que se ha hecho. Es DELIBERADAMENTE CORTA: unas pocas
  frases en lenguaje llano, con énfasis específico en si algo de lo hecho
  CONTRADICE o CAMBIA una cifra, conclusión o frase que el candidato ya
  podría haber dicho o escrito en otro sitio (CV, scorecard, entrevistas
  anteriores, mensajes a Cristina/Carolina). Es la última red de seguridad
  antes de que el usuario cierre la sesión y se le olvide revisar algo
  antes de la entrevista.
---

## Por qué existe esta skill

El riesgo real en este proyecto no es que el código falle — es que el
candidato entre a la entrevista final con Anna Via (Head of AI, Infojobs)
diciendo una cifra o una conclusión que ya cambió en una sesión de
análisis posterior, sin haberse enterado. Ya pasó una vez en el ejercicio
gemelo de Prisma (18/08/2026): una decisión técnica correcta y bien
documentada no llegó a quien la necesitaba en el momento en que la
necesitaba, y el coste fue de credibilidad, no técnico. Aquí el equivalente
sería decir en la entrevista "faltan sinónimos musicales" cuando el propio
análisis ya concluyó lo contrario.

## Cuándo se invoca

Al final de cualquier tarea no trivial sobre el caso (cambiar el notebook,
añadir un informe a `docs/`, tocar una cifra), cuando el usuario escribe
`/supervise` o pide explícitamente un resumen. También es buena práctica
que el propio agente la ofrezca sin que se le pida, al cerrar una tarea
que tocó una conclusión, un número, o algo que el candidato podría repetir
en voz alta en la entrevista.

## Formato — corto, no un informe

No repitas el detalle técnico ya dado durante la tarea. Estructura fija:

```
## Lo que cambió (en una frase por punto)
- ...

## ⚠️ Contradice o cambia algo dicho antes
(Solo si aplica — si no aplica, DI explícitamente "nada de esto contradice
una cifra o conclusión anterior", no omitas la sección en silencio; que
falte sin decir nada es indistinguible de que se te haya olvidado mirar.)
- ...

## Qué necesitas saber tú para la entrevista, no solo el repo
(Lo que cambia lo que puedes decir con seguridad delante de Anna/Carolina,
o una decisión de criterio que es tuya, no del agente — ej. si quieres
usar la cifra del 47% o una más conservadora, o si prefieres no mencionar
el hallazgo de ESCO hasta que te pregunten.)
- ...
```

## Cómo decidir qué es "de verdad relevante" (el filtro)

No todo cambio merece aparecer aquí — eso sería ruido. Un cambio es
relevante si cumple alguno de estos criterios:

1. **Cambia una cifra o conclusión que ya se dijo en el chat, en un
   informe de `docs/`, o que podría repetirse en la entrevista** (el %
   de volumen no-musical, la lista de sinónimos ESCO, la causa raíz).
2. **Contradice o revierte algo hecho antes** — antes de cerrar la tarea,
   pregúntate explícitamente: "¿algo de lo que acabo de hacer hace que una
   frase dicha en una sesión anterior ya no sea cierta?". Si la respuesta
   es sí, va en la sección de arriba, sin excepción, aunque el cambio sea
   una mejora.
3. **Toca algo que ya está en el CV, el scorecard, o un mensaje enviado a
   Cristina/Carolina** — esos documentos ya salieron de tus manos y no se
   pueden editar retroactivamente; si el caso práctico dice algo distinto,
   el candidato necesita saberlo antes de que se lo pregunten.
4. **Deja una decisión pendiente que solo el candidato puede tomar** — no
   una pregunta técnica, una de criterio sobre cómo presentar el hallazgo.

Lo que NO hace falta que aparezca: limpieza de código sin efecto en las
conclusiones, cambios de formato, detalles ya cubiertos en el resumen de
la propia tarea si no cumplen ninguno de los 4 criterios de arriba.
