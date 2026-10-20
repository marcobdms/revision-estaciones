# Revisión de estaciones

Herramienta local (`index.html`, doble clic en Chrome o Edge) para revisar las estaciones de Virtual SecureIT
y volcar el resultado a la hoja de Quip sin buscar filas a mano.

- Sin instalación, sin servidor y sin red.
- Los datos se guardan en el navegador. **Editor → Copia de seguridad** para no perderlos.

## Flujo

1. **Planta** (arriba): Floor 1, Robotics Floor 2/3/4.
2. **Importar hoja** (opcional, recomendado): exporta de Quip la pestaña de esa planta en CSV y cárgala.
   Así cada ficha enseña el serial esperado y compara.
3. En la ficha: login, zona, lado A/B y número de estación. El serial se escribe completo y siempre en mayúsculas.
   - ✓ coincide · ≈ solo cambia una O por un 0 · ✗ no coincide (aparece un campo para explicar qué pasa).
4. Sí/No en etiquetado, anclaje, brida y Vsit. La fecha se pone sola en formato dd/mm/aaaa,
   aunque el portátil tenga el formato de EE. UU.
5. **Guardar y siguiente** deja zona y lado, y limpia el resto para la siguiente estación.

## Exportar a Quip

- **Con hoja importada:** descarga la pestaña completa, con las mismas filas y en el mismo orden, y las estaciones
  revisadas actualizadas. Si una estación revisada no estaba en la hoja, se añade al final.
  Si el serial coincide, se deja como estaba, y los tickets que ya había no se borran.
- **Sin hoja importada:** solo las estaciones revisadas, con las columnas de la hoja.
- Se añade una columna extra al final, **Observaciones (revisión)**, con los comentarios.

Antes de pegar o importar en la hoja real, pruébalo en una copia de la pestaña.

## Editor

Logins, zonas, plantas y modelos son editables (una opción por línea). **La lista de logins empieza vacía**: añádela en Editor la primera vez (se guarda en el navegador y en la copia de seguridad).

## Datos

`ejemplo-hoja.csv` tiene datos inventados para probar. Los CSV y JSON reales están en el `.gitignore`.
