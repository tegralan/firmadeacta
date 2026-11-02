# Firma Remota de Actas de Mediación — Prototipo

Prototipo demostrativo del circuito de firma remota de actas de audiencias de mediación virtual, inspirado en el sistema "Mi Reclamo" (SCJCABA) del Consejo de la Magistratura de la Ciudad Autónoma de Buenos Aires.

> **Importante:** usar exclusivamente datos ficticios. Este prototipo no tiene validez jurídica y no debe alojar información de causas reales.

## Circuito

1. **Redacción y bloqueo:** el/la mediador/a redacta el acta en la página o sube el archivo (PDF, Word .docx o imagen), indica el resultado y la plataforma de la audiencia virtual, y deja constancia de su lectura en audiencia. Al bloquearla se calcula su huella SHA-256 (incluida la del archivo).
2. **Conformidad de las partes**, en dos modos:
   - **Simulado:** enlace y código por SMS simulados dentro de la página (pestaña *Bandeja simulada*).
   - **Correo real (Microsoft 365):** el acta se envía desde la casilla institucional del/la mediador/a (Microsoft Graph `Mail.Send`, mismo registro de aplicación que `ficha`), con un código personal por parte. La parte responde desde su casilla con «PRESTO CONFORMIDAD» y su código; *Buscar respuestas en mi bandeja* (Graph `Mail.Read`) verifica remitente, frase y código y registra la conformidad. Si `Mail.Read` no está autorizado, el/la mediador/a puede registrar la respuesta verificada en Outlook.
3. **Firma digital del mediador/a:** se habilita cuando todas las partes prestaron conformidad.
4. **Acta final:** incluye tabla de conformidades, huellas y QR de verificación. Se puede descargar en PDF y **enviar a las partes** desde la casilla institucional del/la mediador/a (un correo individual por parte, con el PDF y, si corresponde, el documento original).

Además, la página incluye una verificación de autenticidad por huella y un registro probatorio encadenado.

## Varias actas

La barra *Mis actas* permite crear actas nuevas, pasar de una a otra y eliminarlas. Cada mediador/a ve las actas guardadas en su propio navegador. *Volver a editar* desbloquea un acta mientras no se haya enviado a ninguna parte, y un acta en modo simulado puede pasarse a correo real (se descartan los envíos simulados).

## Alcance

Todo funciona en el navegador, sin servidor. En modo simulado, el correo, el SMS y la IP son simulados; la firma digital del/la mediador/a es simulada en ambos modos. El estado se guarda en el `localStorage` del navegador.

La versión institucional requeriría un backend (Python/FastAPI) alojado por la Dirección General de Informática y Tecnología, con envío real de correos y códigos, firma digital remota del mediador (PAdES) e incorporación del acta a EJE.

## Resultados del acta

Tomados de la lista de validación de la planilla *Mediaciones Virtuales* (columna "Resultado de la Mediacion"):

- **Audiencia realizada** (requieren conformidad de las partes): con acuerdo; sin acuerdo; se requiere nueva audiencia; cerrada por decisión del mediador.
- **Audiencia no realizada** (constancia firmada solo por el/la mediador/a): incomparecencia del/la denunciante, del denunciado/a o de ambas partes; se requiere nueva audiencia; por decisión del mediador.

Los estados administrativos (Cancelada, No se realiza por…, Suspendida, Reprogramación, Otros) no generan acta y no se incluyen.

## Configuración de Azure para el modo de correo real

En el registro de aplicación de Azure (CLIENT_ID `b26fea36-887e-4021-8611-08c97de5704e`):

1. Agregar `https://tegralan.github.io/firmadeacta/` como URI de redirección (plataforma *Single-page application*).
2. Agregar el permiso delegado de Microsoft Graph `Mail.Read` (puede requerir consentimiento del administrador del tenant).
