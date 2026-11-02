# Firma Remota de Actas de Mediación — Prototipo

Prototipo demostrativo del circuito de firma remota de actas de audiencias de mediación virtual, inspirado en el sistema "Mi Reclamo" (SCJCABA) del Consejo de la Magistratura de la Ciudad Autónoma de Buenos Aires.

> **Importante:** usar exclusivamente datos ficticios. Este prototipo no tiene validez jurídica y no debe alojar información de causas reales.

## Circuito

1. **Redacción y bloqueo:** el/la mediador/a redacta el acta en la página o sube el archivo (PDF, Word .docx o imagen), indica el resultado y la plataforma de la audiencia virtual, y deja constancia de su lectura en audiencia. Al bloquearla se calcula su huella SHA-256 (incluida la del archivo).
2. **Conformidad de las partes:** cada parte recibe un enlace único en su correo constituido y valida un código enviado a su teléfono.
3. **Firma digital del mediador/a:** se habilita cuando todas las partes prestaron conformidad.
4. **Acta final:** incluye tabla de conformidades, huellas y QR de verificación.

Además, la página incluye una verificación de autenticidad por huella y un registro probatorio encadenado.

## Alcance

Todo funciona en el navegador. El correo, el SMS, la IP y la firma digital son **simulados**. El estado se guarda en el `localStorage` del navegador.

La versión institucional requeriría un backend (Python/FastAPI) alojado por la Dirección General de Informática y Tecnología, con envío real de correos y códigos, firma digital remota del mediador (PAdES) e incorporación del acta a EJE.

## Pendiente

- Reemplazar la lista provisoria `RESULTADOS` (en `index.html`) por los resultados exactos que se usan en EJE y en la planilla de mediaciones.
