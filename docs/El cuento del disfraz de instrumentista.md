# 🎭 El cuento del disfraz de "instrumentista"

*Una forma sencilla de entender el problema del caso práctico, de principio a fin.*

---

## 🏢 Había una vez... una fiesta enorme

Imagina que InfoJobs es una fiesta gigante con millones de personas. Cada persona lleva un cartel colgado del cuello con su oficio escrito: "camarero", "profesor", "guitarrista"...

Para que la fiesta funcione bien, hay un **robot organizador** en la puerta. Su trabajo es mirar el cartel de cada persona y mandarla a la sala correcta: la sala de "hostelería", la sala de "educación", la sala de "música"...

Para saber a qué sala mandar a cada uno, el robot usa un **libro mágico de reglas** que se llama **ESCO**. El libro dice cosas como: *"si el cartel dice 'guitarrista', esa persona va a la sala de Música"*.

---

## 🎻 El disfraz que engaña al robot

Un día, alguien le pregunta al robot: *"¿estás mandando bien a la gente a la sala de Música?"*

El robot mira su lista y dice: "¡Claro! Mira, todos estos llevan carteles que dicen 'guitarrista', 'saxofonista', 'pianista'... todos correctos, sala de Música."

Pero entre la lista, aparecen carteles raros:

- 🏭 "instrumentista maestro planta" (¡trabaja en una fábrica!)
- 🏥 "instrumentista quirófano" (¡trabaja en un hospital, ayudando en operaciones!)
- 🎸 "instrumentista" (a secas, sin decir nada más)

**¿Por qué el robot manda a gente de fábricas y hospitales a la sala de Música?**

Aquí viene la parte importante del cuento: el robot abre su libro mágico ESCO, y en la página de "Música" encuentra una regla que dice literalmente:

> *"Si alguien lleva el disfraz llamado 'instrumentista', mándalo a la sala de Música."*

**¡Esa regla la escribió el propio libro mágico, no se la inventó el robot!** El robot no tiene la culpa — solo obedece el libro.

El problema es que la palabra "instrumentista" es un **disfraz que se ponen tres personas totalmente distintas**:
1. 🎻 El músico que toca un instrumento musical
2. 🏭 El técnico que revisa instrumentos de medición en una fábrica o refinería
3. 🏥 La persona que entrega instrumentos quirúrgicos al cirujano en un quirófano

Los tres llevan el mismo disfraz puesto ("instrumentista"), pero solo uno de ellos es realmente músico. Y el libro mágico ESCO, al escribir su regla, nunca pensó en avisar: *"ojo, este disfraz también se lo ponen otras dos personas — fíjate bien en qué más dice el cartel antes de decidir"*.

---

## 🕵️ Cómo lo descubrimos (la investigación)

No nos creímos la primera sospecha a ciegas. Hicimos como un detective:

1. **Miramos la lista completa** de 210 carteles distintos que el robot había mandado a la sala de Música.
2. **Fuimos directamente a preguntarle al libro mágico ESCO** (su API, en internet) cuál es su lista oficial de disfraces válidos para "Música". Y ahí confirmamos: sí, "instrumentista" está en su lista oficial. No es un error del robot.
3. **Contamos cuánta gente llevaba cada disfraz**, y cuántos de esos carteles tenían pistas de fábrica o de hospital escondidas (palabras como "refinería", "planta", "quirófano", "cirugía").
4. **Hicimos las cuentas:** de toda la gente que el robot mandó a la sala de Música, **casi la mitad (47 de cada 100) en realidad no son músicos**. Y lo más sorprendente: el robot estaba "muy seguro" de mandarlos ahí — no dudaba, simplemente seguía la regla del libro sin mirar el resto del cartel.

---

## 💡 El desenlace: ¿qué hay que arreglar?

Aquí está la trampa del cuento: la primera idea que se te ocurre es *"pues hay que añadir más disfraces de música a la lista"*. Pero investigando, vemos que **ese no es el problema** — la lista de disfraces musicales ya está bastante completa (guitarrista, saxofonista, pianista... todos correctos).

El problema real es el contrario: **el disfraz "instrumentista" está mal etiquetado como "solo para música"**, cuando en realidad lo llevan tres oficios distintos. Y el robot nunca aprendió a **mirar el resto del cartel** antes de decidir.

**La solución propuesta:** enseñarle al robot un truco nuevo, pero solo para los disfraces que ya sabemos que son "trampa" (como "instrumentista"): antes de mandar a alguien a la sala de Música solo por llevar ese disfraz, que se fije también en las demás palabras del cartel. Si ve "refinería" o "planta", que lo mande a Fábrica. Si ve "quirófano" o "cirugía", que lo mande a Hospital. Si no hay ninguna pista rara, entonces sí, que lo mande a Música.

Este truco solo hace falta aplicarlo a los pocos disfraces "trampa" que ya identificamos — no hay que revisar a todo el mundo de la fiesta, solo a los que llevan un disfraz que sabemos que es ambiguo.

**Fin.** 🎬

---

## 🔑 La frase resumen (para decir en la entrevista)

> "El robot no está roto. El diccionario oficial de ESCO dice que 'instrumentista' es sinónimo de músico, y eso es verdad a veces. El problema es que esa misma palabra también la usan técnicos de fábrica e instrumentistas de quirófano, y nadie le enseñó al sistema a distinguir entre los tres mirando el resto del título. Casi la mitad del volumen que hoy se clasifica como músico, en realidad no lo es."
