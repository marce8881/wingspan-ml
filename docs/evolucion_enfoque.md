# Evolución del enfoque: de clasificación supervisada a clustering

Este documento registra cómo y por qué cambió el enfoque del proyecto. Se incluye como parte de la sección de uso de IA y criterio propio, y como referencia metodológica para la Etapa 2 del proyecto mayor.

---

## La idea original

La propuesta inicial planteaba un modelo de **clasificación supervisada**: entrenar un algoritmo (Random Forest, Regresión Logística Multinomial) para predecir a qué tipo de motor pertenece cada carta.

Para eso, era necesario construir una variable objetivo `engine_type` con etiquetas como:

- `motor_huevos`
- `motor_cartas`
- `motor_alimento`
- `motor_depredador`

Esas etiquetas se construirían manualmente a partir de reglas heurísticas sobre las columnas del dataset: si una carta tenía `lay egg` en el texto de poder y alta presencia en `Grassland`, se etiquetaba como `motor_huevos`.

---

## Por qué se descartó

Al desarrollar las reglas de etiquetado, surgió el problema central: **las etiquetas reflejarían mi interpretación del juego, no el comportamiento real de las cartas en una partida**.

El riesgo concreto es el sesgo de diseño. Si yo decido que "lay egg" implica motor de pradera, el modelo aprende esa regla — no descubre nada. Solo repite con más matemática lo que yo ya sabía. El modelo no estaría encontrando patrones en los datos; estaría confirmando supuestos previos.

Además, el proyecto tiene una visión a largo plazo: la Etapa 2 consiste en registrar **partidas reales** y analizar qué cartas y combinaciones generan más puntos en la práctica. Construir etiquetas manuales hoy y usarlas para entrenar un modelo supervisado crearía una deuda metodológica difícil de corregir: ¿cómo comparar los resultados de un modelo entrenado sobre etiquetas de diseño con los patrones que emerjan de datos reales de partida?

La conclusión fue que la variable objetivo correcta no existe todavía. Y construir una artificial para cumplir el requisito de "modelo supervisado" sería exactamente lo que la estadística busca evitar.

---

## Cómo se redefinió el proyecto

En lugar de forzar una variable objetivo, se decidió trabajar con la única variable que sí existe en los datos de forma objetiva y completa: el texto de poder (`Power text`).

La pregunta de investigación cambió de:

> "¿A qué tipo de motor pertenece esta carta?" *(supervisado, requiere etiquetas)*

a:

> "¿Qué grupos naturales emergen de los textos de poder, y cómo se relacionan con las mecánicas del juego?" *(no supervisado, los grupos emergen de los datos)*

El enfoque pasó a ser **clustering no supervisado**:

1. Vectorizar `Power text` con TF-IDF para convertir el texto en representación numérica.
2. Reducir dimensionalidad con PCA para eliminar ruido antes del clustering.
3. Aplicar K-Means y DBSCAN para descubrir grupos naturales.
4. Describir cada grupo usando las variables reales del dataset (`Color`, `Predator`, `Flocking`, hábitat, puntos, costo).

Con este enfoque, los arquetipos emergen del texto — no de reglas previas. Si el modelo agrupa bien, es porque las cartas comparten patrones reales en su diseño, no porque yo los haya codificado.

---

## Conexión con la Etapa 2

Los clusters encontrados en este análisis servirán como hipótesis de trabajo para la Etapa 2, no como verdad establecida.

Cuando exista el histórico de partidas reales, la pregunta será: ¿los arquetipos que emergen del texto de poder se corresponden con los motores que realmente generan más puntos? Esa es la validación que ningún modelo supervisado sobre etiquetas manuales podría ofrecer.

El cambio de enfoque no fue una concesión al tiempo disponible. Fue la decisión metodológicamente correcta dado el estado actual de los datos.
