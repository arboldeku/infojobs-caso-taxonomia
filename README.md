# Caso práctico - Clasificación de "músico/música" en la taxonomía ESCO de InfoJobs

Análisis de calidad de la clasificación automática a la posición ESCO "músico/música",
realizado como parte del proceso de selección para el puesto de Data Analyst for
Taxonomies (Python & SQL) en InfoJobs (proceso gestionado por Ricaris).

## Contexto

InfoJobs usa la taxonomía [ESCO](https://esco.ec.europa.eu/) para clasificar ofertas de
empleo y experiencias de candidatos/as. Este ejercicio audita la calidad de esa
clasificación para la posición "músico/música": se dispone de un fichero con los
títulos originales que un modelo predictivo asignó a esa posición, junto con el score
medio de confianza y el número de filas (ofertas/experiencias) que tenía cada título.

## Estructura del repositorio

```
.
├── data/
│   └── raw/
│       └── normalized_value_focus.csv   # fichero original entregado (UTF-16, separado por tabulador)
├── docs/
│   └── propuesta_prueba_tecnica.pdf     # enunciado original del caso
├── notebooks/
│   └── analisis_taxonomia_musico.ipynb  # análisis completo, conclusiones y propuesta de automatización IA
├── requirements.txt
└── README.md
```

## Preguntas que responde el análisis

1. ¿Qué conclusiones se pueden sacar de la calidad en la clasificación a "músico/música"?
2. ¿Qué problema se puede detectar y cuál puede ser el motivo?
3. ¿Se podrían sugerir nuevos sinónimos muy usados en títulos que actualmente no se tienen en cuenta?

Y, como entregable adicional: una propuesta de automatización basada en IA para una
tarea manual identificada durante el análisis.

## Cómo ejecutar

```bash
python -m venv .venv
source .venv/bin/activate  # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/analisis_taxonomia_musico.ipynb
```

## Autor

Albert Bañeres - [albertbaneresdata.com](https://www.albertbaneresdata.com/)
