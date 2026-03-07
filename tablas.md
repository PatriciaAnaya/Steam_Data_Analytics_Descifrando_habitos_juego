# Base de Datos de Jugadores de Steam - Diccionario y Estructura

Este documento define la estructura, el origen y el propósito de las variables recolectadas a través de la API de Steam (Steamworks Web API) para el análisis de comportamiento de jugadores.

## 1. Identificadores y Metadatos del Juego
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`appid`** | `Integer` | Identificador único del juego en el ecosistema Steam. Es la clave primaria para cruzar con datos de la tienda. |
| **`name`** | `String` | Nombre oficial del título tal como aparece en la biblioteca del usuario. |
| **`img_icon_url`** | `String` | Hash hexadecimal que identifica el icono del juego. Se usa para construir la URL del recurso gráfico. |
| **`content_descriptor`** | `List / CSV` | Etiquetas de categorías de contenido (ej. "Violencia", "Lenguaje Fuerte"). Útil para segmentación por género. |

## 2. Información del Usuario y Actividad
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`idsuser_id`** | `String (64-bit)` | El **SteamID64** único del jugador. Se debe tratar como *String* para evitar pérdida de precisión numérica. |
| **`playtime_forever`** | `Integer` | Tiempo total acumulado de juego en **minutos** desde que el usuario adquirió el título. |
| **`playtime_2weeks`** | `Integer` | Tiempo de juego en los últimos 14 días en **minutos**. Es el principal indicador de "Engagement" actual. |

## 3. Atributos de Funcionalidad
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`has_community_visible_stats`** | `Boolean` | Flag que indica si el perfil del usuario permite la lectura pública de sus estadísticas detalladas. |
| **`has_leaderboards`** | `Boolean` | Indica si el juego posee tablas de clasificación globales integradas con Steam. |

## 4. Métricas de Progresión y Logros
| Variable | Tipo de Dato | Descripción |
| :--- | :--- | :--- |
| **`achieved`** | `Integer` | Número total de logros que el usuario ha desbloqueado satisfactoriamente. |
| **`total_achievements`** | `Integer` | Número total de logros disponibles para ese `appid` específico. |
| **`completion_percentage`** | `Float` | Porcentaje de progreso calculado. Fórmula: $\frac{achieved}{total\_achievements} \times 100$. |


# Diccionario de Datos: `juegos_maestro`

Este conjunto de datos actúa como la **Tabla de Dimensión de Juegos** principal en el modelo relacional de Power BI. Proporciona el contexto detallado para cada título y se relaciona con la tabla de actividad del usuario (logs/playtime) mediante el campo `appid`.

| Columna | Tipo de Dato | Descripción | Regla de Negocio / Limpieza |
| :--- | :--- | :--- | :--- |
| **`appid`** | `Integer` | ID único del juego en Steam. | **Clave Primaria**. No admite duplicados. |
| **`name`** | `String` | Nombre oficial del videojuego. | Nombre tal cual aparece en la tienda. |
| **`developer`** | `String` | Estudio desarrollador. | Se extrae únicamente el primer estudio de la lista. |
| **`price`** | `Decimal` | Precio de venta del juego. | Los juegos "Gratis" o sin precio definido se marcan como **0**. |
| **`genre`** | `String` | Género principal asignado. | Enriquecido con **Ollama**. |
| **`is_free`** | `Boolean` | Indicador de gratuidad. | `True` si el juego es Free-to-Play. |
| **`metacritic_score`** | `Integer` | Puntuación de la crítica (0-100). | El valor **0** representa la ausencia de puntuación oficial. |
| **`total_recommendations`** | `Integer` | Conteo de reseñas positivas. | Métrica de popularidad histórica en Steam. |
| **`playtime_hours`** | `Decimal` | Tiempo total de juego en horas. | Calculado como: `playtime_forever / 60`. |
| **`adult_descriptorids`** | `Boolean` | Flag de contenido sensible. | **True** si contiene descriptores de contenido adulto; **False** si es familiar. |
| **`country`** | `Boolean` | Pais residente de los estudios desarrolladores | Enriquecido con **Ollama** |
---