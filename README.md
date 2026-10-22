# Revisión de estaciones

Herramienta local (`index.html`, doble clic en Chrome o Edge) para revisar las estaciones de Virtual SecureIT
y volcar el resultado a la hoja de Quip sin buscar filas a mano.

- Sin instalación, sin servidor y sin red.
- Los datos se guardan en el navegador. **Editor → Copia de seguridad** para no perderlos.
- Guía para agentes de IA: [`CLAUDE.md`](CLAUDE.md).

## Flujo

1. **Importar hoja**: en Quip, exporta el documento a Excel (`.xlsx`) y cárgalo. Se cargan a la vez todas las plantas
   (Floor 1, Robotics Floor 2/3/4). También vale el CSV de una pestaña, pero las fechas salen ambiguas.
2. **Planta** (arriba). Cada planta guarda su ficha a medias: puedes cambiar de planta sin perder nada.
3. En la ficha: login, lado **A / B / Otra** y número de estación (se sugieren las de la hoja).
   **Otra** es para estaciones con nombre (`SLAM_WEST_02`, `VERNIER3 P2`).
4. Al reconocer la estación, se rellena sola con lo que pone la hoja: zona, serial, modelo, comprobaciones,
   tickets y comentarios. Comprueba el serial con la etiqueta y corrige lo que haya cambiado.
   - ✓ coincide · ≈ solo cambia una O por un 0 · ✗ no coincide (explícalo en Comentarios).
5. Las comprobaciones son las de la pestaña: 4 en Robotics (etiquetado, anclada, brida, Vsit), 3 en Floor 1.
6. La fecha de revisión es la del portátil, en formato dd/mm/aaaa.
7. **Guardar y siguiente** deja zona y lado, y limpia el resto. **Limpiar** pide confirmación.

## Llevarlo a Quip

- **Copiar para Quip**: copia las filas de la pestaña (con las revisadas actualizadas). En Quip, clic en la celda
  **A4** de esa pestaña y Ctrl+V.
- **Exportar CSV**: descarga la pestaña completa con la misma cabecera, filas y orden.
- Solo cambian las estaciones revisadas. Si el serial coincide, se deja como estaba. Las estaciones nuevas van al final.

Antes de pegar en la hoja real, pruébalo en una copia de la pestaña.

## Editor

Logins, zonas (solo si no hay hoja), plantas y modelos son editables (una opción por línea).
**La lista de logins empieza vacía**: añádela la primera vez.

## Datos

- `ejemplo-hoja.csv`: datos inventados para probar.
- Los datos reales **no están en el repo**: se importan desde tu portátil (Excel exportado de Quip). CSV, Excel y JSON de copia de seguridad están en el `.gitignore`.
