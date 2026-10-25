# Control Integral de Taller — Guía completa para Android

Aplicación web móvil en HTML5, CSS3 y JavaScript vanilla. Diseño inspirado en Material/Google: navegación inferior en Android, botones táctiles grandes, tarjetas claras y formularios adaptables. Incluye manifiesto y service worker para permitir instalarla como aplicación web (PWA).

## Antes de empezar: qué funciona y qué falta configurar
- Puedes abrir la app y probar sus pantallas sin datos inventados.
- Sin conectar Google Sheets, los registros se guardan solo en el navegador que uses. No es multiusuario.
- Para compartir datos entre tu Android y el personal del taller debes conectar una hoja de Google Sheets a través de Apps Script.
- El adaptador incluido es de ejemplo y no implementa autenticación completa. No conectes información sensible de la empresa hasta restringir y probar el acceso en backend.
- WhatsApp abre un chat con el texto listo; tú debes pulsar Enviar. En el caso de Grupo, selecciona el grupo manualmente.

## Paso 1 — Descargar y descomprimir
1. Descarga `Control_Integral_de_Taller.zip`.
2. En Windows, clic derecho → Extraer todo.
3. Conserva juntos `index.html`, `styles.css`, `app.js`, `manifest.webmanifest`, `sw.js`, `icon.svg`, `netlify.toml` y la carpeta `google-apps-script`.
4. No abras ni edites el ZIP directamente.

## Paso 2 — Crear el repositorio en GitHub
1. Entra a https://github.com e inicia sesión.
2. Pulsa `+` → `New repository`.
3. Nombre: `control-integral-taller`.
4. Selecciona `Private` si no quieres que el código sea público. La privacidad del repositorio no protege automáticamente los datos de una API pública.
5. Pulsa `Create repository`.
6. Dentro del repositorio pulsa `Add file` → `Upload files`.
7. Arrastra los archivos y carpetas que extrajiste. Asegúrate de que `index.html` quede en la raíz del repositorio, no dentro de una segunda carpeta `Control_Integral_de_Taller`.
8. Pulsa `Commit changes`.

## Paso 3 — Publicar en Netlify
1. Entra a https://app.netlify.com.
2. Inicia sesión y elige `Add new site` / `Import an existing project`.
3. Selecciona GitHub y autoriza el acceso al repositorio.
4. Escoge `control-integral-taller`.
5. En ajustes de despliegue, deja `Build command` vacío.
6. En `Publish directory`, escribe `.` (un punto).
7. Pulsa `Deploy site`.
8. Espera a que el estado diga que se publicó. Abre la URL `https://...netlify.app` que Netlify asigne.
9. Si ves una página vacía o 404, revisa que `index.html` esté en la raíz del repositorio y vuelve a desplegar.

## Paso 4 — Instalarla en Android
Hazlo desde Chrome en el teléfono, no desde la vista previa del editor:
1. Abre la URL de Netlify.
2. Espera a que cargue y prueba navegar por Inicio, Equipos y Órdenes.
3. Pulsa el menú de Chrome `⋮`.
4. Busca `Instalar aplicación` o `Añadir a pantalla de inicio` (el texto depende de la versión de Chrome).
5. Confirma la instalación.
6. Vuelve a la pantalla principal de Android y abre `Control Taller`.
7. Comprueba que la navegación esté abajo y que los botones se puedan tocar sin hacer zoom.
8. Si no aparece la opción de instalar, comprueba que la URL usa HTTPS y que `manifest.webmanifest`, `sw.js` e `icon.svg` se han publicado.

## Paso 5 — Preparar Google Sheets
1. Abre Google Sheets y crea una hoja nueva llamada `Control Integral de Taller - Datos`.
2. Conserva el enlace de la hoja. El ID es la cadena entre `/d/` y `/edit` en la URL.
3. Haz una copia de seguridad antes de importar cualquier registro existente.
4. En la hoja, abre `Extensiones` → `Apps Script`.
5. Abre el archivo `google-apps-script/Code.gs` incluido en el ZIP y copia todo su contenido al editor de Apps Script, reemplazando el código de ejemplo.
6. En la primera línea de configuración cambia `REEMPLAZA_CON_ID_REAL` por el ID de tu hoja. No pongas la URL completa: solo el ID.
7. Guarda el proyecto.
8. El código puede crear las pestañas básicas al recibir la primera lectura. Los nombres esperados son `Equipos`, `OrdenesTrabajo`, `Compras`, `MaterialesOT` y `ContactosReportes`.
9. Si ya tienes una hoja de AppSheet, no borres la original. Primero crea una copia y compara encabezados e identificadores. Este adaptador no migra por sí solo los registros antiguos.

## Paso 6 — Publicar Apps Script (solo para prueba)
1. En Apps Script pulsa `Deploy` → `New deployment`.
2. Selecciona tipo `Web app`.
3. Elige ejecutar como tu cuenta si las políticas de tu organización lo permiten.
4. Define quién puede acceder según las políticas de la empresa.
5. Pulsa `Deploy` y completa los permisos solicitados.
6. Copia la URL de la aplicación web que termina normalmente en `/exec`.
7. **No asumas que la URL es privada.** Si configuras acceso para cualquiera, otros podrían intentar leer o modificar datos. El ejemplo no valida la identidad del usuario ni aplica permisos por rol. No lo uses para información operativa sensible con acceso anónimo.
8. Para un uso real con datos empresariales, detente aquí y pide que se implemente autenticación/autorización en backend, o configura un acceso restringido a usuarios autorizados y prueba que un usuario no autorizado no pueda leer ni escribir.

## Paso 7 — Conectar la web con Apps Script
Solo después de resolver el acceso:
1. Abre `app.js` en GitHub.
2. Cerca del inicio encontrarás `const CONFIG={appsScriptUrl:""};`.
3. Dentro de las comillas coloca la URL `/exec` desplegada.
4. Guarda con `Commit changes`.
5. Netlify volverá a publicar automáticamente.
6. Abre la web y revisa que el indicador cambie de `Modo local` a `Google Sheets`.
7. Crea un registro de prueba no sensible, actualiza la página y comprueba que siga apareciendo.
8. Prueba la misma URL en otro navegador/dispositivo autorizado y confirma que ambos ven el mismo registro.
9. Si falla, abre Apps Script → `Executions` y revisa el error; no pegues aquí tokens ni información confidencial.

## Paso 8 — Registrar los equipos reales
1. Entra en `Equipos` → `Agregar equipo`.
2. Registra ficha, compañía, código, área, marca, modelo y chasis cuando se conozcan.
3. Guarda cada equipo una sola vez. Evita duplicados de ficha/código.
4. El estado inicial del equipo sirve como estado base.
5. Las OT abiertas pueden cambiar el estado mostrado si el impacto operativo se marca `No opera` o `Fuera de servicio`.
6. Esta versión no interpreta automáticamente el texto libre de una OT; el impacto debe seleccionarse expresamente en el campo `Impacto operativo`.

## Paso 9 — Crear la primera OT (solo analista)
1. Ve a `Órdenes` → `Nueva OT — Analista`.
2. Introduce el número real de OT, compañía, equipo y fecha/hora de solicitud.
3. Selecciona el impacto operativo si ya está confirmado.
4. Deja sin completar los datos técnicos que todavía no te han entregado.
5. Guarda.
6. En Seguimiento de OT deben aparecer tres estados de avance: Analista, Mecánico y Encargado.
7. Usa `Apartado mecánico` y `Apartado encargado` para completar los datos conforme los recibas. Ambos modifican la misma OT.
8. Las OT abiertas aparecen antes que las cerradas; dentro de las abiertas se ordenan por prioridad Alta, Media y Baja. Si la prioridad aún no se ha asignado, la OT queda después de las prioridades conocidas.

## Paso 10 — Materiales y compras
1. Registra los materiales de una OT en `Materiales por OT`: código, descripción, cantidades solicitadas, utilizadas y devueltas.
2. Registra cada solicitud real en `Compras pendientes`.
3. Cambia su estado conforme avance: Pendiente, En proceso, Comprada, Recibida o Cancelada.
4. La app no crea compras automáticamente al abrir una OT, porque no debe inventar piezas no solicitadas.

## Paso 11 — Contactos y WhatsApp
1. En `Contactos`, registra el rol `Mecánico`, `Encargado` y `Grupo`.
2. Escribe el número del destinatario con código de país, solo dígitos; por ejemplo, un número de República Dominicana empieza por `1` seguido del número de diez dígitos.
3. Para Grupo, el enlace puede abrir WhatsApp sin un número preseleccionado; elige el grupo correcto manualmente.
4. En Seguimiento de OT pulsa `WhatsApp mecánico`, `WhatsApp encargado` o `WhatsApp grupo`.
5. Revisa el texto, el destinatario y los pendientes antes de pulsar Enviar.
6. Si no existe contacto para ese rol, WhatsApp puede abrir la pantalla de selección; el texto debe estar precargado cuando el navegador lo permita.

## Paso 12 — Validación final antes de operar
Completa esta lista con una OT de prueba y datos no sensibles:
- [ ] La web abre desde el ícono de Android.
- [ ] La app se puede usar con una sola mano y la navegación inferior no tapa formularios.
- [ ] Un equipo nuevo aparece en el catálogo único.
- [ ] Una OT nueva solo solicita inicialmente los datos del analista.
- [ ] La OT aparece en Seguimiento.
- [ ] Los botones de mecánico y encargado editan la misma OT.
- [ ] La prioridad ordena las OT abiertas.
- [ ] Una OT cerrada aparece debajo de las abiertas.
- [ ] El impacto `No opera` hace que el equipo aparezca arriba en rojo.
- [ ] Materiales y compras quedan vinculados a la OT correcta.
- [ ] WhatsApp genera el texto adecuado y el usuario confirma el envío.
- [ ] Google Sheets refleja los registros y dos dispositivos autorizados ven los mismos datos.
- [ ] Un usuario no autorizado no puede acceder a los datos.

## Importante
No se puede prometer terminar una conexión segura multiusuario solo subiendo archivos: hay que configurar Google, permisos, despliegue y probar. El ZIP es el proyecto de interfaz y un adaptador básico; autenticación, permisos por rol, firma digital y fotos/evidencias requieren trabajo adicional antes de producción.
