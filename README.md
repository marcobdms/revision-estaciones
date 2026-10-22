# Revisión de estaciones

Herramienta local (`index.html`, doble clic en Chrome o Edge) para revisar las estaciones de Virtual SecureIT
y volcar el resultado a la hoja de Quip sin buscar filas a mano.

- Sin instalación, sin servidor y sin red.
- Los datos se guardan en el navegador. **Editor → Copia de seguridad** para no perderlos.
- Guía para agentes de IA: [`CLAUDE.md`](CLAUDE.md).

## Flujo

1. **Planta** (arriba): Floor 1, Robotics Floor 2/3/4.
2. **Importar hoja** (opcional, recomendado): exporta de Quip la pestaña de esa planta en CSV y cárgala.
   Así cada ficha enseña el serial esperado, los tickets que ya tiene y compara.
   Acepta la cabecera en dos filas (`Ticket 1/2/3`, `Comentarios`) y zonas escritas distinto (`Rios` = `Ríos`).
3. En la ficha: login, zona, lado **A / B / Otra** y número de estación. El serial se escribe completo y siempre en mayúsculas.
   - **Otra** sirve para estaciones con nombre libre (`VERNIER3 P2`, `P2_ICQA_AA2_01`).
   - ✓ coincide · ≈ solo cambia una O por un 0 · ✗ no coincide (aparece un campo para explicar qué pasa).
4. Sí/No en etiquetado, anclaje, brida y Vsit. La fecha se pone sola en formato dd/mm/aaaa,
   aunque el portátil tenga el formato de EE. UU.
5. Hasta 3 tickets nuevos. Los que ya estaban en la hoja se muestran y se conservan.
6. **Guardar y siguiente** deja zona y lado, y limpia el resto para la siguiente estación.

## Exportar a Quip

- **Con hoja importada:** descarga la pestaña completa, con las mismas filas y en el mismo orden (incluida la cabecera
  original), y las estaciones revisadas actualizadas.
  - Si el serial coincide, se deja como estaba. `Zona` y `Estación` no se reescriben.
  - Los tickets nuevos van al primer hueco libre de Ticket 1–3; los que ya había no se borran.
  - Los comentarios nuevos se añaden a los existentes.
  - Las fechas de la hoja escritas como mm/dd/aaaa que son inequívocas (p. ej. `10/21/2026`) se pasan a dd/mm/aaaa.
  - Si una estación revisada no estaba en la hoja, se añade al final.
- **Sin hoja importada:** solo las estaciones revisadas, con las columnas de la hoja.

Antes de pegar o importar en la hoja real, pruébalo en una copia de la pestaña.

## Editor

Logins, zonas, plantas y modelos son editables (una opción por línea). **La lista de logins empieza vacía**: añádela en Editor la primera vez (se guarda en el navegador y en la copia de seguridad). Las zonas nuevas que aparezcan en una hoja importada se añaden solas.

## Datos

- `ejemplo-hoja.csv`: datos inventados para probar.
- `datos/robotics-floor-2.csv`: exportación real de Robotics Floor 2 (con seriales y logins). **Mantén el repositorio privado.**
  El resto de CSV y los JSON de copia de seguridad siguen en el `.gitignore`.
