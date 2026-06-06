# Decisiones sobre variables del dataset

Este documento registra las decisiones tomadas sobre las variables del dataset durante el análisis, incluyendo el rol de cada variable, las exclusiones y sus razones.

El proyecto usa un enfoque de **clustering no supervisado**. No hay variable objetivo (`y`) ni conjunto de features (`X`) en el sentido supervisado. Las variables se dividen en tres roles:

| Rol | Descripción |
|---|---|
| **Variable de análisis** | `Power text` — se vectoriza con TF-IDF y es el input del clustering |
| **Variables de enriquecimiento** | Se usan para describir el perfil de cada cluster, no como input del modelo |
| **Variables excluidas** | No aportan información útil o no están procesadas en esta versión |

---

## 1. Mecánica de costo de comida: columnas `/` y `*`

### Contexto

El costo de comida de una carta puede ser fijo, flexible o comodín. El dataset lo registra en tres capas:

| Columna | Qué representa |
|---|---|
| `Invertebrate`, `Seed`, `Fish`, `Fruit`, `Rodent`, `Nectar` | Cantidad de fichas de tipo específico requeridas |
| `Wild (food)` | Fichas comodín que aceptan cualquier tipo de alimento |
| `/ (food cost)` | Fichas con opción entre dos tipos (elige uno u otro) |
| `* (food cost)` | Fichas de tipo completamente libre (cualquier alimento) |
| `Total food cost` | Suma total de fichas necesarias, sin importar el tipo |

### Ejemplos

- **Costo fijo:** `Seed=1, Invertebrate=1, Total food cost=2` → pagas exactamente 1 semilla + 1 invertebrado.
- **Costo con `/`:** `/ (food cost)=1, Total food cost=1` → pagas 1 ficha que puede ser de uno u otro tipo específico.
- **Costo con `*`:** `* (food cost)=1, Total food cost=1` → pagas 1 ficha de cualquier tipo (máxima flexibilidad).

### Decisión

`/ (food cost)` y `* (food cost)` fueron **excluidas** por alto porcentaje de nulos. `Total food cost` se mantiene como **variable de enriquecimiento** porque captura el costo de entrada y permite comparar el peso económico de cada cluster.

**Implicación no modelada:** la flexibilidad del costo (`/` y `*`) es una señal relevante para el motor de alimento — un ave con `*` es más fácil de jugar porque no depende de un dado específico. Queda como mejora potencial.

---

## 2. Mecánica de dirección del pico: columna `Beak direction`

### Contexto

`Beak direction` indica hacia qué lado mira el pico del ave en la carta (`L` = izquierda, `R` = derecha). Afecta la acción **Roba cartas de ave** en el Humedal.

| Valor | Fuente de la carta |
|---|---|
| `L` ← | Roba del mazo boca abajo — carta aleatoria |
| `R` → | Roba de la bandeja boca arriba — el jugador elige |

Las aves con pico `R` son más valiosas en un motor de cartas porque permiten seleccionar la carta más conveniente para el combo.

### Decisión

`Beak direction` fue **excluida** en esta versión. No entra al clustering (que trabaja sobre texto) ni se usa como variable de enriquecimiento. Puede incorporarse fácilmente en análisis futuros como binaria (`R=1, L=0`) para enriquecer el perfil del cluster de motores de cartas.

---

## 3. Clasificación de variables

### Variable de análisis principal

| Variable | Uso |
|---|---|
| `Power text` | Se limpia y vectoriza con TF-IDF (`max_features=200`, `ngram_range=(1,3)`, `min_df=3`). Es el único input de K-Means y DBSCAN. |

### Variables de enriquecimiento

No entran al modelo. Se usan en la sección de interpretación para describir el perfil estadístico de cada cluster.

| Variable | Qué aporta al perfil del cluster |
|---|---|
| `Color` | Tipo de activación del poder (marrón, rosa, blanco, verde azulado, amarillo) |
| `Predator` | Indica si el ave tiene mecánica de depredación |
| `Flocking` | Indica si el ave tiene mecánica de bandada (tuck desde la mano) |
| `Forest` | Presencia en el hábitat Bosque |
| `Grassland` | Presencia en el hábitat Pradera |
| `Wetland` | Presencia en el hábitat Humedal |
| `Egg limit` | Capacidad máxima de huevos — mide especialización en motor de pradera |
| `Victory points` | Puntos base — mide el peso estratégico del cluster |
| `Total food cost` | Costo de despliegue — mide la exigencia económica del cluster |

### Variables excluidas durante la limpieza

| Variable | Razón |
|---|---|
| `Scientific name` | Identificador taxonómico, sin valor analítico |
| `Flavor text` | Texto decorativo sin relación con mecánicas |
| `Fan Art flavor text` | Texto decorativo de edición especial |
| `Fan Art Pack?` | Metadato editorial |
| `Fan art beak direction` | Metadato editorial de edición especial |
| `Swift Start` | Mecánica de variante de juego rápido, no relevante para motores |
| `Automa ban` | Indica si la carta está prohibida en modo solitario |
| `/ (food cost)` | Flexibilidad del costo — excluida por alto porcentaje de nulos |
| `* (food cost)` | Comodín del costo — excluida por alto porcentaje de nulos |
| `Beak direction` | Variable `L/R` — no usada en esta versión (ver sección 2) |
| `Common name` | Identificador del ave, sin valor analítico para el clustering |

---

## 4. Mejoras potenciales identificadas

| Mejora | Impacto esperado | Complejidad |
|---|---|---|
| Reemplazar TF-IDF por embeddings semánticos (Word2Vec, BERT) | Capturar sinónimos (`gain`, `take`, `get`) que TF-IDF trata como tokens distintos | Alta |
| Incluir `Beak direction` como variable de enriquecimiento | Enriquecer el perfil del cluster de motores de cartas | Baja |
| Incluir `/ (food cost)` y `* (food cost)` con imputación | Capturar la flexibilidad del costo en el perfil de cada cluster | Media |
| Construir etiquetas de arquetipos con jugadores expertos | Permite calcular métricas de clasificación y validar el clustering cuantitativamente | Alta |
| Analizar expansiones por separado | El juego base aporta 174 cartas frente a 25 de cada pack promo; puede sesgar los clusters | Media |
