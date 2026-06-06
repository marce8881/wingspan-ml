# Wingspan ML — Análisis estratégico y clustering de cartas

Proyecto final del Diplomado en Transformación Digital Empresarial.

## Pregunta de investigación

¿Qué grupos naturales emergen de los textos de poder de las cartas de Wingspan, y cómo se relacionan esos grupos con el tipo de activación, las mecánicas especiales y el hábitat del ave?

## Dataset

Fuente: [BoardGameGeek](https://boardgamegeek.com/) — base de datos de cartas de Wingspan (juego base + expansiones).

- 707 cartas de aves
- 65 variables por carta: hábitat, costo de comida, tipo de nido, puntos de victoria, texto de poder, región geográfica, entre otras.
- Archivo: `data/raw/wingspan-20260128.xlsx`

## Estructura del proyecto

```
wingspan-ml/
├── data/
│   └── raw/                    ← dataset original sin modificar
├── docs/                       ← propuesta, guía del curso y descripción del dataset
├── notebooks/
│   └── wingspan_analysis.ipynb ← análisis principal (EDA → modelos → clustering → conclusiones)
├── requirements.txt
└── README.md
```

## Cómo ejecutar

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/marce8881/wingspan-ml.git
   cd wingspan-ml
   ```

2. Crear entorno virtual e instalar dependencias:
   ```bash
   python -m venv .venv
   source .venv/bin/activate   # En Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. Abrir el notebook:
   ```bash
   jupyter notebook notebooks/wingspan_analysis.ipynb
   ```

## Enfoque metodológico

- **Tipo:** Aprendizaje no supervisado — Clustering sobre texto vectorizado
- **Técnica de representación:** TF-IDF sobre el campo `Power text`
- **Reducción de dimensionalidad:** PCA (varianza explicada) + t-SNE (visualización 2D)
- **Algoritmos comparados:** K-means vs DBSCAN
- **Interpretación:** perfil de cada cluster según `Color`, `Predator`, `Flocking` y hábitat

## Autora

Marcela Cadena — Diplomado en Transformación Digital Empresarial, 2026
