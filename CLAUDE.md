# Revisión de estaciones — guía para agentes

Herramienta de un solo archivo (`index.html`, JS vanilla, sin dependencias, sin red, sin build) para revisar los
escáneres de las estaciones de BCN1 (Virtual SecureIT Standards) y volcar el resultado a la hoja de Quip vía CSV.
Se usa con doble clic en Chrome o Edge desde un portátil corporativo (formato regional EE. UU., VPN Cisco).
Proyecto hermano: `scanner-sequence` (misma filosofía: un solo HTML, `localStorage`, español).

## Reglas del proyecto

- **Un solo `index.html`.** No añadir frameworks, bundlers, CDN ni llamadas de red. Debe funcionar offline desde `file://`.
- Idioma de la interfaz y comentarios: **español**. Estilo del código: ES5 en una IIFE (`var`, `function`), sin módulos.
- Persistencia: `localStorage` con la clave `station-review-v1` (estado completo: listas, revisiones, hojas importadas).
  Si cambias la forma del estado, mantén compatibilidad con copias antiguas (ver `load()` y el restaurar copia).
- **Fecha siempre `dd/mm/aaaa`** generada a mano (`todayStr`), nunca con `toLocaleDateString` (el portátil es EE. UU.).
- El serial se escribe completo y en mayúsculas (`snNorm`). Comparación: ✓ igual, ≈ solo difiere O/0, ✗ distinto.
- No hay tests automáticos. Para probar, ver «Cómo probar» abajo.

## Modelo de datos

- `state.records[]`: una revisión por estación y planta (`floor`, `zona`, `lado`, `numero`, `estacion`, `serial`,
  `coincide`, `modelo`, `fecha`, `login`, las 4 preguntas `etiquetado|anclada|brida|vsit` = `'Si'|'No'`,
  `ticket1..3`, `comentario`).
- `state.sheets[planta]`: hoja de Quip importada (`pre` filas previas, `hdrRows` filas de cabecera, `headers`
  fusionadas, `rows`, `map` columna→índice).
- Lado `A`/`B` + número da `A-2104`. Lado **`Otra`** = nombre libre (`VERNIER3 P2`, `P2_ICQA_AA2_01`).

## Formato real de la hoja (importante)

El CSV exportado de Quip (ver `datos/robotics-floor-2.csv`) **no es limpio**:
- Primera fila basura (`eifjcbvdu...`) y **cabecera en dos filas**: la 2ª (`Ticket 1, Ticket 2, Ticket 3, Comentarios`)
  tiene vacías las 4 primeras celdas. `importSheet` las fusiona y conserva ambas para exportar igual que vino.
- Zonas escritas distinto: `Rios` vs `Ríos`, `Mar (col. A17)`. El cruce usa `zoneKey` (sin tildes ni paréntesis).
- Fechas mezcladas dd/mm y mm/dd (`10/21/2026`). `fixDate` solo corrige las **inequívocas** (un valor > 12).
- Hay seriales con minúsculas y erratas en la hoja; no se corrigen sin que el revisor lo confirme.

## Reglas de exportación (no romper)

Al exportar con hoja importada: mismas filas y orden; solo se tocan las estaciones revisadas.
- No se sobrescriben `Zona`/`Estación` de filas existentes.
- Si el serial coincide, se deja como estaba.
- Tickets **nunca se pisan**: los nuevos van al primer hueco libre de Ticket 1..3; si no caben, a Comentarios.
- Los comentarios nuevos se **añaden** a los existentes (` · `), sin duplicar.
- Si la hoja no tiene columna `Comentarios`, se añade al final `Observaciones (revisión)`.

## Cómo probar

1. Abrir `index.html`, planta «Robotics Floor 2», **Importar hoja** → `datos/robotics-floor-2.csv` (o `ejemplo-hoja.csv`).
2. Casos a comprobar: `Ríos B-2223` (debe encontrarse con serial `17090B3843`), lado **Otra** + `VERNIER3 P2`,
   revisar y **Exportar CSV** → abrir y verificar que cabecera (2 filas), tickets y comentarios existentes siguen.
3. Prueba headless posible en Windows con Edge (`msedge --headless=new --dump-dom`) inyectando un script de prueba
   en una copia temporal; no hay Node en el equipo.

## Confidencialidad

`datos/` contiene seriales y logins reales de la planta. Mantener el repositorio **privado**. No subir capturas ni el
manual de origen. `*.json` (copias de seguridad) sigue ignorado.

## Git

El portátil corporativo bloquea `api.github.com` y no tiene `git`/`gh` en consola; el push se hace desde Visual Studio.
