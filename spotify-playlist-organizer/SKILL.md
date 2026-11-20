---
name: spotify-playlist-organizer
description: Usa esta skill cuando Daniel pida organizar, ordenar, limpiar, deduplicar, agrupar o crear playlists en Spotify por mood, género, energía, BPM o uso (estudio, entreno, set de DJ, casa).
---

# Organizador de playlists en Spotify

## Paso 0: revisa qué puede hacer el conector
Antes de prometer cualquier acción, revisa qué herramientas de Spotify están disponibles en esta sesión.
- Si no puedes leer las canciones de una playlist, no inventes su contenido. Pide a Daniel que la exporte a CSV (por ejemplo con Exportify) y trabaja con ese archivo.
- Si no puedes modificar una playlist, entrega la lista final para que la cree o la edite manualmente en Spotify.
- Dile siempre qué puedes y qué no puedes hacer antes de empezar.

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
