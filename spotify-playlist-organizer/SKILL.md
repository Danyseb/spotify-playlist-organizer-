---
name: spotify-playlist-organizer
description: Usa esta skill cuando Daniel pida organizar, ordenar, limpiar, deduplicar, agrupar o crear playlists en Spotify por mood, género, energía, BPM o uso (estudio, entreno, set de DJ, casa).
---

# Organizador de playlists en Spotify

## Paso 0: revisa qué puede hacer el conector
Antes de prometer cualquier acción, revisa qué herramientas de Spotify están disponibles en esta sesión.
- Lo verificado en noviembre de 2026: el conector puede **buscar** (incluye las playlists de Daniel), **crear playlists nuevas** desde una descripción (requiere Premium) y **guardar o quitar** elementos de la biblioteca. **No puede** leer las canciones de una playlist existente ni agregar, quitar o reordenar sus canciones.
- Usa la búsqueda para ubicar la playlist: el resultado trae nombre, dueño, URI y `can_edit_items` (si Daniel puede editarla). No trae las canciones.
- Si no puedes leer las canciones de una playlist, no inventes su contenido. Pide a Daniel que la exporte a CSV con Exportify (ver abajo) y trabaja con ese archivo.
- Si no puedes modificar una playlist, entrega la lista final para que la edite en Spotify, u ofrece crear una playlist nueva con el conector (aclara que el conector elige las canciones desde una descripción y puede no respetar tu lista exacta).
- Dile siempre qué puedes y qué no puedes hacer antes de empezar.

## Leer un CSV de Exportify
Exportify es una web gratuita, sin instalación: https://exportify.net. Daniel entra, conecta su cuenta de Spotify, elige la playlist y descarga el CSV.

Columnas que trae el CSV:
`Track URI, ISRC, Track Name, Album Name, Artist Name(s), Release Date, Duration (ms), Popularity, Explicit, Added By, Added At, Genres, Record Label, Danceability, Energy, Key, Loudness, Mode, Speechiness, Acousticness, Instrumentalness, Liveness, Valence, Tempo, Time Signature`

Reglas al leerlo:
- Lee el archivo completo con código y reporta cuántas filas tiene antes de proponer nada.
- **Columnas vacías:** las de audio (Energy, Tempo, Key, etc.) pueden venir vacías, porque Spotify restringió esos datos. Si vienen vacías, dilo y no las estimes sin marcarlo como estimación.
- **Duplicados:** primero por `Track URI`; después por `ISRC` (misma grabación en otro álbum o remaster). Propón cuál conservar.
- **Duración:** convierte `Duration (ms)` a minutos:segundos.
- **BPM:** `Tempo` es el BPM. Redondéalo a entero.
- **Energía:** `Energy` va de 0 a 1. Multiplícalo por 10 para usar la escala 1-10 de la skill `djing-setlist`.
- **Género:** usa la columna `Genres`. Si está vacía, aplica las reglas de clasificación por género de abajo o marca "sin clasificar".
- **Tonalidad a Camelot:** `Key` es la nota (0 = C … 11 = B; -1 = sin detectar) y `Mode` es 1 = mayor, 0 = menor. Convierte así:

| Key | Nota | Mayor (Mode 1) | Menor (Mode 0) |
|---|---|---|---|
| 0 | C | 8B | 5A |
| 1 | C#/Db | 3B | 12A |
| 2 | D | 10B | 7A |
| 3 | D#/Eb | 5B | 2A |
| 4 | E | 12B | 9A |
| 5 | F | 7B | 4A |
| 6 | F#/Gb | 2B | 11A |
| 7 | G | 9B | 6A |
| 8 | G#/Ab | 4B | 1A |
| 9 | A | 11B | 8A |
| 10 | A#/Bb | 6B | 3A |
| 11 | B | 1B | 10A |

- Para un set de DJ, prioriza las keys de Rekordbox si Daniel las tiene; las de Spotify son una referencia.
- Al terminar, entrega la lista nueva también como CSV con `Track URI`, para que Daniel la importe o la reconstruya en Spotify.

## Antes de tocar nada
1. Identifica la playlist objetivo por nombre exacto. Si hay varias parecidas, pregunta cuál.
2. Lee el contenido completo: nombre, número de tracks, artistas, duración y fecha de agregado. Si la lectura está incompleta (paginación cortada, CSV parcial), dilo y no continúes como si estuviera completa.
3. Confirma la acción antes de modificar:
   - Reordenar o eliminar duplicados: pide confirmación.
   - Crear una playlist nueva: propón nombre y lista, y pide visto bueno.
   - Nunca borres tracks sin confirmación explícita.

## Operaciones
- **Limpiar:** detectar duplicados (mismo track en versión remaster, live o explicit) y proponer cuál conservar.
- **Agrupar:** separar por mood, género, energía o uso. Propón las subplaylists y sus nombres antes de crearlas.
- **Ordenar:** por BPM o energía para uso en DJ, o por curva emocional para escuchar de corrido.
- **Crear desde cero:** a partir de una idea ("música para programar sin letra, 90 minutos"), buscar tracks y armar la lista.
- **Completar:** sugerir tracks similares a los existentes, con una razón breve.

## Reglas de clasificación por género
- Usa el género del **álbum** y de **cada artista** del track. Combínalos y elimina duplicados.
- Si no hay género en ninguna fuente, clasifica como "sin clasificar". No adivines.
- Cuando un track tenga varios géneros, asígnalo al género principal y menciona los secundarios solo si Daniel lo pide.

## Reglas de guardado
- Al crear o llenar una playlist, agrega canciones en lotes de máximo 100.
- Después de cada lote, confirma cuántos se agregaron y si hubo errores.
- Si un lote falla, no reintentes en silencio: reporta qué lote falló y cuántas canciones quedaron pendientes.

## Criterios de organización
- **Mood:** energía alta, media o baja; tono (oscuro, luminoso, nostálgico).
- **Uso:** estudio (sin letra o letra mínima, ritmo estable), entreno (BPM alto, subidas claras), casa (fondo, volumen bajo), DJ (BPM y key Camelot, ver skill `djing-setlist`).
- **Duración:** cuando el uso lo requiera (25 minutos de estudio, 60 de entreno).
- **Energía y BPM:** no inventes estos datos. Si no están disponibles, dilo y ofrece estimarlos, marcados explícitamente como estimación.

## Formato de salida
Para propuestas de organización, usa tabla:

| Playlist propuesta | Criterio | Tracks | Duración aprox. |
|---|---|---|---|

Antes de ejecutar, muestra un resumen:
- Qué se va a crear, modificar o quitar.
- Cuántos tracks se afectan.
- Qué no se puede hacer con las herramientas disponibles, si aplica.

Después de ejecutar, confirma en una o dos líneas qué cambió.

## Límites honestos
- No inventes metadata (BPM, key, energía, género). Si no está disponible, dilo.
- No accedas a playlists o cuentas que no sean de Daniel.
- Si una operación no es posible con el conector, dilo y propone el paso manual en la app de Spotify.

## Qué no hacer
- No cambies el nombre de una playlist sin pedirlo.
- No reordenes una playlist entera si solo se pidió limpiar duplicados.
- No agregues tracks explícitos a una playlist marcada como familiar o similar sin preguntar.
- No uses código de terceros ni tokens de Spotify en la conversación.
