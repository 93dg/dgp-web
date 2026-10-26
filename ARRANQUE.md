# ARRANQUE — leer antes de hacer nada (cualquier chat, cualquier modelo)

Subproyecto: **dgp** · Base: **Supabase qtnqupcfgooazjzkpmtx (DGP)** (datos en el esquema `personal`, que hace de prefijo) · Web: **GitHub 93dg/dgp-web**, publicada con GitHub Pages en **https://93dg.github.io/dgp-web/**

Cómo funcionamos: Claude trabaja con el conector de Supabase (datos y memoria del método) y con el repositorio de GitHub (la web). El chat no es memoria. Ver ARQUITECTURA.md.

La memoria de este proyecto vive en la base de datos, no en el chat. Si el chat es nuevo, largo o resumido, da igual: se sigue esto.

1. **Abrir caso:**
   - `select preparar_tarea('dgp', 'orden', array['Sección'], 'ejecucion');`
   - Leer la salida ENTERA y llamar a `confirmar_guia(caso_id, clave, reglas que aplican)`.
2. **Antes de tocar una sección:** `select memoria_seccion(caso_id, 'Sección');`. Leerla entera.
3. **Seguridad del 80 %:** por debajo, una sola pregunta con la propuesta escrita.
4. **Bandeja en cada mensaje:** `select bandeja('dgp');`. Decir al final si hay algo que hacer.
5. **Escribir datos:** antes, `set local metodo.caso_id = '<caso_id>';`.
6. **Lo que digo se guarda en el mismo turno:** `anotar_hecho(tema, hecho, palabras)`. Si es un patrón, también `anotar_regla(...)`.
7. **Nunca enviar nada en mi nombre** sin enseñar el borrador y mi «sí».
8. **Publicar:** `./herramientas/publicar.sh VERSION "mensaje" CASO_ID`. Después, `cerrar_tarea(...)`.
9. **Cada respuesta termina** con `VX.YY · Método v1.0 · DGP`.

Reglas propias de este subproyecto (además de las comunes):
- DGP es el centro privado de todo lo personal de Daniel, salvo las contraseñas. Es independiente de EXG: no se toca `93dg/exg-sistema`.
- Los datos viven en el esquema `personal`. Cada tabla lleva seguridad por filas: solo el correo del dueño (`auth.jwt() ->> 'email'`) puede leer y escribir. En la web solo la clave pública; ningún secreto en el repositorio.
- Las secciones se definen como datos: `personal.secciones` (nombre, tipo, configuración) y `personal.registros` (datos en jsonb). Una sección nueva es una fila nueva, sin tocar la web. Los tipos especiales (`imdb`, `deuda`) tienen su propia vista en `index.html`.
- Las hojas de cálculo de Google se revisan una a una: sección nueva si vale, datos a Supabase y la hoja original a la papelera solo con el sí de Daniel.
- Deuda Diana: saldo previo de 13.000 € a 19/11/2025; el alquiler suma 250 € cada mes; los gastos se reparten por comercio (compartido al 50 %, mío, solo de Diana o ignorar). Lo sin clasificar no cuenta.
- Las herramientas de `herramientas/` solo auditan la pantalla de acceso, porque la web exige contraseña. Falta una sesión de prueba (tarea pendiente en la bandeja).
