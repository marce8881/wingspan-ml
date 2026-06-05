# Diccionario del dataset: wingspan-20260128.xlsx

Dataset fuente: BoardGameGeek (BGG) — hoja `Birds`.  
Total de columnas: 65 | Filas de datos reales: ~907 cartas de aves (juego base + expansiones).

> **Nota de carga:** las filas 1 y 2 del Excel son estadísticas internas del archivo, no cartas. Siempre cargar con `skiprows=[1, 2]`.

---

## Identificación del ave

| Columna | Tipo | Descripción |
|---|---|---|
| `Common name` | Texto | Nombre común del ave en inglés. Identificador principal. |
| `Scientific name` | Texto | Nombre científico (género y especie). Solo referencia taxonómica. |
| `Set` | Categórica | Expansión de origen de la carta. Valores: `core` (juego base), `european`, `oceania`, `asia`, `americas`, `promoUS`, entre otros. |

---

## Atributos del poder

| Columna | Tipo | Descripción |
|---|---|---|
| `Color` | Categórica | Color del poder de la carta — indica cuándo se activa. `brown`: al activar el hábitat. `pink`: una vez entre turnos (turno del oponente). `white`: al jugar la carta (una sola vez). `teal`: al final de cada ronda. Sin color: sin poder activo. |
| `Power text` | Texto libre | Descripción completa del poder del ave tal como aparece en la carta. Base para derivar `engine_type`. |
| `Predator` | Binaria (X/vacío) | El ave es depredadora: puede cazar otras aves del mazo y solaparlas para ganar puntos. |
| `Flocking` | Binaria (X/vacío) | El ave forma bandadas: solapa cartas de la mano del jugador (1 punto por carta solapada al final). |
| `Bonus card` | Binaria (X/vacío) | El ave permite robar o interactuar con cartas de bonificación. |

---

## Puntuación y características físicas

| Columna | Tipo | Descripción |
|---|---|---|
| `Victory points` | Numérica (0–9) | Puntos base que vale el ave al final de la partida. |
| `Nest type` | Categórica | Tipo de nido del ave. Valores: `platform`, `bowl`, `cavity`, `ground`, `wild`, `star`. Relevante para cartas de bonificación que puntúan por tipo de nido. |
| `Egg limit` | Numérica | Máximo de huevos que puede almacenar el ave. Los huevos actúan como moneda para jugar aves en columnas 2 a 5, y valen 1 punto cada uno al final. |
| `Wingspan` | Numérica (cm) | Envergadura del ave en centímetros. Relevante para bonificaciones de aves grandes (>65 cm) o pequeñas (≤30 cm). |

---

## Hábitats

Indican en qué filas del tablero puede jugarse el ave. Un ave puede pertenecer a uno, dos o los tres hábitats.

| Columna | Tipo | Descripción |
|---|---|---|
| `Forest` | Binaria (X/vacío) | El ave puede jugarse en el Bosque. Hábitat de motor de alimento. |
| `Grassland` | Binaria (X/vacío) | El ave puede jugarse en la Pradera. Hábitat de motor de huevos. |
| `Wetland` | Binaria (X/vacío) | El ave puede jugarse en el Humedal. Hábitat de motor de robo de cartas. |

---

## Costo de alimento

Cantidad de fichas de cada tipo de alimento necesarias para jugar la carta. Nulo significa que ese tipo no se requiere.

| Columna | Tipo | Descripción |
|---|---|---|
| `Invertebrate` | Numérica | Fichas de invertebrado (gusano) requeridas. |
| `Seed` | Numérica | Fichas de semilla requeridas. |
| `Fish` | Numérica | Fichas de pescado requeridas. |
| `Fruit` | Numérica | Fichas de fruta requeridas. |
| `Rodent` | Numérica | Fichas de roedor requeridas. |
| `Nectar` | Numérica | Fichas de néctar requeridas (exclusiva de la expansión Oceania). |
| `Wild (food)` | Numérica | Fichas comodín: acepta cualquier tipo de alimento. |
| `/ (food cost)` | Numérica | Fichas con opción entre dos tipos específicos (el jugador elige uno u otro). Mecánica de flexibilidad. |
| `* (food cost)` | Numérica | Fichas de tipo completamente libre (cualquier alimento). Máxima flexibilidad. |
| `Total food cost` | Numérica | Suma total de fichas de alimento necesarias para jugar el ave, independientemente del tipo. |

---

## Dirección del pico y mecánicas especiales

| Columna | Tipo | Descripción |
|---|---|---|
| `Beak direction` | Binaria (L/R) | Dirección del pico en la ilustración. `L` (izquierda): roba del mazo boca abajo (aleatorio). `R` (derecha): roba de la bandeja boca arriba (el jugador elige). Afecta poderes de tuck y robo en el Humedal. |
| `Swift Start` | Binaria | Indica si la carta tiene reglas especiales para la variante de inicio rápido del juego. |
| `Automa ban` | Binaria | Indica si la carta está prohibida en el modo solitario (Automa). No relevante para análisis multijugador. |
| `Flavor text` | Texto | Frase descriptiva o dato curioso sobre el ave. Sin valor para el modelo. |

---

## Distribución geográfica

Indican en qué regiones del mundo vive el ave realmente. Relevantes para la carta de bonificación `Cartographer`.

| Columna | Tipo | Descripción |
|---|---|---|
| `North America` | Binaria (X/vacío) | El ave habita en América del Norte. |
| `Central America` | Binaria (X/vacío) | El ave habita en América Central. |
| `South America` | Binaria (X/vacío) | El ave habita en América del Sur. |
| `Europe` | Binaria (X/vacío) | El ave habita en Europa. |
| `Asia` | Binaria (X/vacío) | El ave habita en Asia. |
| `Africa` | Binaria (X/vacío) | El ave habita en África. |
| `Oceania` | Binaria (X/vacío) | El ave habita en Oceanía. |

---

## Metadatos de Fan Art

Columnas relacionadas con la edición especial de arte de fans. Sin valor analítico.

| Columna | Descripción |
|---|---|
| `Fan Art Pack?` | Indica si existe una versión Fan Art de esta carta. |
| `Fan Art flavor text` | Texto de sabor de la versión Fan Art. |
| `Fan art beak direction` | Dirección del pico en la versión Fan Art. |

---

## Compatibilidad con cartas de bonificación

Estas columnas indican qué tan bien encaja el ave con cada carta de bonificación del juego. Los valores son proporciones (0.0 a 1.0) que representan la probabilidad de que esa ave sea seleccionada cuando la carta de bonificación correspondiente está en juego.

| Columna | Condición de la carta de bonificación |
|---|---|
| `Anatomist` | Aves con partes del cuerpo en su nombre (back, beak, belly, bill, breast, etc.) |
| `Cartographer` | Aves que habitan en al menos 3 regiones geográficas distintas |
| `Historian` | Aves cuyo nombre común incluye un nombre de persona o lugar histórico |
| `Photographer` | Aves con envergadura ≤ 30 cm (aves pequeñas fotogénicas) |
| `Backyard Birder` | Aves con menos de 4 puntos de victoria |
| `Bird Bander` | Aves con nido tipo cuenco (`bowl`) |
| `Bird Counter` | Aves de pradera (`Grassland=1`) |
| `Bird Feeder` | Aves que comen semillas (`Seed > 0`) |
| `Diet Specialist` | Aves que solo comen un tipo de alimento |
| `Enclosure Builder` | Aves con nido tipo cavidad (`cavity`) |
| `Endangered Species Protector` | Aves con 0 puntos de victoria |
| `Falconer` | Aves depredadoras (`Predator=1`) |
| `Fishery Manager` | Aves que comen pescado (`Fish > 0`) |
| `Food Web Expert` | Aves que comen 3 o más tipos distintos de alimento |
| `Forester` | Aves de bosque (`Forest=1`) |
| `Large Bird Specialist` | Aves con envergadura > 65 cm |
| `Nest Box Builder` | Aves con nido tipo cavidad (`cavity`) en bosque o pradera |
| `Omnivore Expert` | Aves que aceptan comodín de alimento (`Wild (food) > 0`) |
| `Passerine Specialist` | Aves paseriformes (orden Passeriformes) |
| `Platform Builder` | Aves con nido tipo plataforma (`platform`) |
| `Prairie Manager` | Aves de pradera con nido tipo suelo (`ground`) |
| `Rodentologist` | Aves que comen roedores (`Rodent > 0`) |
| `Small Clutch Specialist` | Aves con límite de huevos ≤ 2 |
| `Viticulturalist` | Aves que comen fruta (`Fruit > 0`) |
| `Wetland Scientist` | Aves de humedal (`Wetland=1`) |
| `Wildlife Gardener` | Aves de pradera que comen semillas o fruta |
