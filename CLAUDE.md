# Revisión de estaciones — guía para agentes

Herramienta de un solo archivo (`index.html`, JS vanilla, sin dependencias, sin red, sin build) para revisar los
escáneres de las estaciones de BCN1 (Virtual SecureIT Standards) y volcar el resultado a la hoja de Quip.
Se usa con doble clic en Chrome o Edge desde un portátil corporativo (formato regional EE. UU., VPN Cisco).
Proyecto hermano: `scanner-sequence` (misma filosofía: un solo HTML, `localStorage`, español).

## Reglas del proyecto

- **Un solo `index.html`.** No añadir frameworks, bundlers, CDN ni llamadas de red. Debe funcionar offline desde `file://`.
- Idioma de la interfaz y comentarios: **español**. Estilo del código: ES5 en una IIFE (`var`, `function`), sin módulos.
  Excepción ya usada: `Promise`, `DecompressionStream`, `TextDecoder` para leer el `.xlsx` (Chrome/Edge actuales).
- Persistencia: `localStorage` con la clave `station-review-v1` (listas, revisiones, hojas importadas y borradores).
  Si cambias la forma del estado, mantén compatibilidad en `adopt()` (lo usan la carga y Restaurar copia).
- **Fecha de revisión = fecha del portátil**, siempre `dd/mm/aaaa` generada a mano (`todayStr`),
  nunca con `toLocaleDateString` (el portátil es EE. UU.). Se pone al guardar, también al corregir una revisión.
- El serial se guarda en mayúsculas (`snNorm`). Comparación con la hoja: ✓ igual, ≈ solo difiere O/0, ✗ distinto.
- Toda acción que borre datos pide confirmación (Limpiar con ficha a medias, borrar fila, borrar planta, restaurar, reimportar).
- No hay tests automáticos. Ver «Cómo probar».

## La hoja de Quip

Se importa el **Excel exportado de Quip** (no está en el repo) (todas las pestañas a la vez) o el CSV de una sola pestaña.
Pestañas del libro: `Robotics Floor 2`, `Floor 1`, `Robotics Floor 3`, `Robotics Floor 4` (se usan: el nombre coincide
con una planta del Editor), y `Cows`, `Guia de uso`, `Configuración Base…` (se ignoran).

Estructura de cada pestaña de planta:
- Fila 1: título/basura. Filas 2–3: **cabecera en dos filas** (la 3 tiene `Ticket 1/2/3` y `Comentarios`).
  `importRows` las fusiona para mapear columnas y conserva ambas para exportar igual.
- Columnas: `Zona`, `Estación`, `Modelo`, `S/N Scanner`, **comprobaciones Sí/No**, `¿Fecha Ultima Revisión?`,
  `¿Quien lo revisó?`, `Ticket 1..3`, `Comentarios`.
- **Las comprobaciones cambian por planta**: Robotics 2/3/4 tienen 4 (etiquetado, anclada, Brida, Vsit);
  Floor 1 tiene 3 (etiquetado con Dymo, anclada, «¿Etiquetadas y Virtual Secure IT aplicado?» = Vsit).
  Se detectan por posición (columnas entre S/N y Fecha) y se reconocen con `QUESTIONS`/`QUESTION_PRIORITY`.
  Columnas desconocidas en ese tramo se usan igual con su cabecera como texto.
- En el Excel las fechas reales son números (`46316` → `21/10/2026`, `excelDate`) y algunos seriales son números
  (`2423382885.0` → `2423382885`). En el CSV de Quip esas mismas fechas salen como `mm/dd/aaaa`: ambiguo,
  `fixDate` solo corrige si un valor es > 12. **Preferir siempre el Excel.**
- Zonas escritas distinto (`Rios`/`Ríos`, `Mar (col. A17)`): el cruce usa `zoneKey` (sin tildes ni paréntesis).
- Estaciones sin formato lado-número (`SLAM_WEST_02`, `VERNIER3 P2`): lado **Otra** con texto libre.

## Ficha

- Al escribir la estación (lado + número, con sugerencias de la hoja en un `datalist`) se busca en la pestaña de la
  planta en **cualquier zona** (`lookup`) y se **rellena todo** con lo que pone la hoja: zona, serial, modelo,
  comprobaciones, tickets y comentarios (`checkPrefill`). El revisor confirma o corrige.
- La ficha a medias se guarda **por planta** en `state.drafts`: cambiar de planta o recargar no la borra.
- Las zonas que se muestran son las de la hoja de esa planta; sin hoja, las del Editor.

## Exportar (no romper)

Con hoja importada (`buildExport`): mismas filas y orden, cabecera original; solo se tocan las estaciones revisadas.
- No se reescriben `Zona`/`Estación` de filas existentes. Si el serial coincide, se deja como estaba (respeta minúsculas).
- Tickets y comentarios se escriben **tal cual están en la ficha** (vienen precargados de la hoja, así que no se pierden).
- Estaciones que no estaban en la hoja se añaden al final.
- **Exportar CSV** descarga la pestaña completa. **Copiar para Quip** copia solo las filas de datos (TSV) para pegarlas
  en Quip desde la celda `A4`.

## Cómo probar

1. Abrir `index.html`, **Importar hoja** → Excel de Quip exportado en el portátil (o `ejemplo-hoja.csv` para probar sin datos reales).
2. Robotics Floor 2, lado B, `2316` → debe rellenarse zona Mar, serial `2011N0934`, Vsit = No, tickets INC12817744 e INC12825776.
3. Cambiar a Floor 1 y volver: la ficha sigue. Floor 1 muestra 3 comprobaciones.
4. Guardar, Exportar CSV y comprobar que cabecera (2 filas), tickets y comentarios siguen.
5. Prueba headless en Windows: Edge `--headless=new --dump-dom` sobre una copia temporal con un script inyectado
   antes del cierre de la IIFE (no hay Node ni Python en el equipo).

## Confidencialidad

El Excel de Quip y cualquier CSV real tienen seriales, logins y enlaces internos de la wiki: **no se suben al repo** (`.gitignore`). Se importan en local desde el portátil. No subir capturas ni el manual.

## Git

El portátil corporativo bloquea `api.github.com`; `github.com` sí funciona. Git está instalado en
`%LOCALAPPDATA%\Programs\Git`. El push lo hace el usuario (VS Code o `git push`).
