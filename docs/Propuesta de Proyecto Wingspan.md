# **Propuesta de Proyecto Final \- Diplomado en Transformación Digital Empresarial**

**Tema:** Análisis estratégico y clustering de cartas del juego de mesa *Wingspan*

## **1\. Objetivo, estrategia del dataset y visión a largo plazo**

Desarrollar un proyecto de Machine Learning reproducible aplicado a un problema analítico y estratégico.

* **Visión del proyecto (Etapa 1 de un proyecto mayor):** Este entregable académico constituye la **Etapa 1** de un proyecto estadístico personal a largo plazo desarrollado en conjunto con mi esposo (también estadístico). Como ávidos fanáticos de los juegos de mesa, siendo *Wingspan* nuestro gran favorito, nuestro objetivo final es extraer un histórico de partidas reales y analizar empíricamente qué motores otorgan mayor cantidad de puntos. Este proyecto de clase sentará las bases metodológicas, de limpieza de datos y de modelado inicial para esa meta mayor.
* **Cumplimiento de volumen:** El dataset principal contiene **707 cartas de aves** con 65 variables cada una (juego base + todas las expansiones), superando ampliamente el mínimo de 500 filas requerido. Las variables incluyen texto de poder, hábitat, costo de alimento, tipo de nido, puntos de victoria, mecánicas especiales (Predator, Flocking) y tipo de activación (Color).

## **2\. Enfoque metodológico: clustering no supervisado**

Se aplica un enfoque de **Aprendizaje No Supervisado (Clustering)**.

* **Pregunta de investigación:** *¿Qué grupos naturales emergen de los textos de poder de las cartas de Wingspan, y cómo se relacionan esos grupos con el tipo de activación, las mecánicas especiales y el hábitat del ave?*
* **Justificación del enfoque:** Los textos de poder contienen la esencia estratégica de cada carta. Al vectorizarlos con TF-IDF y aplicar clustering, es posible descubrir arquetipos de diseño que las reglas del juego no explicitan, sin necesidad de construir etiquetas manuales que introduzcan sesgo.
* **Pipeline técnico:**
  1. Vectorización con **TF-IDF** (`max_features=200`, `ngram_range=(1,3)`, `min_df=3`, `sublinear_tf=True`)
  2. Reducción de dimensionalidad con **PCA** (50 componentes, ~83% de varianza retenida)
  3. Visualización exploratoria con **t-SNE** (2D, solo para graficar — no como input de modelos)
  4. Comparación de algoritmos: **K-Means** vs **DBSCAN**
* **Selección de hiperparámetros:** método del codo + silhouette score para K-Means; gráfica k-distance para DBSCAN.
* **Sin train/test split:** el clustering es no supervisado y usa la totalidad del mazo.
* **Métricas de evaluación:** silhouette score (K-Means: 0.1745 con k=10, válido en NLP).

## **3\. Entregables y repositorio (portafolio profesional)**

El proyecto se entrega como un portafolio en un **repositorio público de GitHub**. El repositorio cumple con este checklist:

* Enlace público y funcional.
* Historial de desarrollo con al menos **3 commits significativos** con mensajes descriptivos.
* Archivo `README.md` con la descripción del proyecto e instrucciones de ejecución.
* Archivo `requirements.txt` con todas las dependencias necesarias.
* Jupyter Notebook limpio, ejecutado sin errores, estructurado y acompañado de texto en celdas Markdown.

## **4\. Estructura obligatoria del Jupyter Notebook (rúbrica)**

El desarrollo en el cuaderno sigue la siguiente distribución porcentual:

1. **Problema y datos (15%):** Planteamiento de la pregunta estratégica. Carga reproducible con `skiprows=[1, 2]`, EDA del campo `Power text` (longitud, palabras frecuentes, nulos) y limpieza del texto.
2. **Modelado y análisis (35%):** Vectorización TF-IDF, reducción PCA, visualización t-SNE, entrenamiento de K-Means y DBSCAN, justificación del número de clusters con método del codo y silhouette score.
3. **Interpretación (25%):** Perfil de cada cluster según vocabulario TF-IDF, distribución de `Color`, `Predator`, `Flocking` y hábitat. Se incluyen al menos **3 visualizaciones** (scatter t-SNE, heatmap de mecánicas, barras apiladas por color), cada una con su título e interpretación analítica textual.
4. **Conclusiones (15%):** Respuesta a la pregunta de investigación, comparación K-Means vs DBSCAN, limitaciones del análisis y recomendación concreta al jugador para el draft inicial.
5. **Uso de IA (10%):** Celda Markdown que documenta el uso de Claude Code y NotebookLM: prompts representativos, respuestas recibidas y ajustes realizados con criterio propio.

## **5\. Logística y presentación final**

* **Registro:** Inscripción del equipo en el Google Sheets del curso.
* **Video explicativo:** Grabación de un video de máximo 5 minutos demostrando el impacto del análisis.
* **Regla de oro de la presentación:** Enfoque en el *storytelling* estratégico y los hallazgos (ej. "El algoritmo identificó 10 arquetipos de juego sin conocer las etiquetas oficiales, y confirmó que las cartas de bonificación son las más costosas y de mayor retorno"), sin leer el código del cuaderno.
