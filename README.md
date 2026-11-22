# Tablero HD-FL · Hunter Douglas

Tablero de control de pedidos, faltantes, garantías, inventarios y análisis MIX. Reemplaza el Excel de control: las bases se cargan en la pestaña Carga y los informes se actualizan solos.

Página publicada con GitHub Pages; las bases y las cuentas se guardan en Google Sheets. Cada persona entra con su propia cuenta (correo @hdlao.com), que un administrador aprueba.

- **Página:** https://camilobernalsilva-alt.github.io/hd-tablero-fl/
- **Repositorio:** https://github.com/camilobernalsilva-alt/hd-tablero-fl
- `index.html`: la página completa (incluye las librerías, pero ya no las tablas de consulta). GitHub Pages la publica desde la raíz.
- `Code.gs` y `Referencias.gs`: los dos archivos de Google Apps Script. No se suben a GitHub: van dentro de la hoja de cálculo "HD Tablero HD-FL – base de datos".

## Cómo funciona

- Quien carga una base en la pestaña **Carga** la sube al servidor comprimida; el resto del equipo la recibe sola, cada 3 minutos o con **Sincronizar ahora** (pestaña Perfil).
- Las tablas de zonas, estados y dealers 72 horas y los comentarios de Pending se combinan entre varias personas. Las demás bases quedan con la última carga.
- Las tablas de consulta originales (zonas con su gerente, estados, dealers y equivalencias de telas) viven en `Referencias.gs`. El navegador las pide al servidor después del ingreso y las borra al cerrar sesión.
- Cada navegador conserva una copia local para abrir rápido. Al cerrar sesión, esa copia se borra de ese equipo.
- En la hoja de cálculo, las pestañas ocultas `D_...` son las bases (no se editan a mano); **Indice** dice quién subió cada una y cuándo; **Bitacora** guarda las cargas; **Auditoria** guarda códigos enviados, verificaciones, ingresos, intentos fallidos, aprobaciones, acciones de administración y descargas a Excel; **Usuarios** guarda los perfiles (la contraseña solo se guarda cifrada).

## Primer ingreso y acceso del equipo

1. Abre la página, elige **Crear cuenta** y regístrate con `camilo.bernal@hdlao.com`: es el administrador principal y queda activo de inmediato.
2. Pulsa **Subir todo lo de este navegador** en la pestaña Perfil si ya tenías bases cargadas.
3. Comparte el enlace y, por un canal privado, la clave del equipo. Cada persona crea su cuenta con esa clave, su correo **@hdlao.com** y una contraseña personal.
4. Al crear la cuenta llega un **código de 6 dígitos** al correo @hdlao.com (vence en 10 minutos). La cuenta solo se crea cuando la persona lo escribe.
5. Después la cuenta queda **pendiente**. Un administrador la aprueba en Perfil > Administración (botón Aprobar) y recién entonces la persona puede ingresar.

## Cambios en el código del servidor

La primera vez que se use la verificación por correo, ejecuta la función `probarCorreo` desde el editor de Apps Script (botón Ejecutar) y autoriza el permiso de enviar correos; llega un correo de prueba a la cuenta del administrador. Los códigos salen de la cuenta de Google dueña del script (límite diario de Google: unos 100 correos en cuentas gratuitas). Si algo falla con los correos, `REQUIERE_VERIFICACION = false` en `Code.gs` vuelve al registro anterior.

Pega el nuevo `Code.gs` y, si hay que actualizarlas, `Referencias.gs` en Apps Script (dos archivos del mismo proyecto) y publica: **Implementar > Gestionar implementaciones > lápiz > Versión: Nueva versión > Implementar**. La URL no cambia.

## Seguridad

- Sin cuenta no se ve ningún dato: ni las bases ni las tablas de consulta, que ya no están en el HTML público.
- Solo se aceptan correos @hdlao.com; lo valida el servidor. Para crear la cuenta hay que escribir el código que llega al correo (5 intentos por código, 10 minutos de vigencia, un envío por minuto y 5 por hora), y luego un administrador debe aprobarla. Mientras no se escribe el código, la cuenta no existe en la hoja.
- Tras 5 intentos fallidos, el correo queda bloqueado 10 minutos. La clave del equipo se bloquea para todos durante 10 minutos tras 15 intentos fallidos seguidos.
- Permisos por base: en `Code.gs`, constante `PERMISOS` (por defecto todos ven y cargan, como antes). Borrar bases es solo de administradores.
- Los Excel descargados llevan el correo de quien los descargó, y cada descarga queda en la hoja Auditoria.
- Los textos que escribe una persona (nombre, archivo) no pueden convertirse en fórmulas de la hoja.
- Si la clave del equipo se filtra, cámbiala en la propiedad `CLAVE` de Apps Script y entrégala de nuevo.
- Limitación: quien ya tiene acceso puede descargar a Excel o tomar capturas; solo se disuade con la marca y la auditoría. GitHub Pages y Apps Script no permiten restringir por IP.
