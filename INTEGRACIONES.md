# Alquimia — próximas integraciones

## Agenda y turnos
La interfaz de agenda ya contempla:
- modalidad presencial, virtual y a domicilio;
- selección de fecha y horario;
- bloqueo visual del horario seleccionado;
- datos del tutor, correo, WhatsApp, mascota y motivo;
- endpoint preparado en el frontend: `/api/appointments`.

### Integración definitiva pendiente
Para que el bloqueo sea global y persistente, la agenda necesita una fuente de datos compartida (por ejemplo Supabase/Postgres o un sistema de agenda externo) y una cuenta de calendario de Alquimia. El flujo definitivo será:

1. consultar disponibilidad real;
2. reservar el slot de forma atómica;
3. crear el evento en el calendario de Alquimia;
4. enviar correo de recepción/confirmación;
5. programar recordatorio 24 horas hábiles antes;
6. incluir botones para confirmar o rechazar asistencia;
7. liberar el slot si el turno es rechazado/cancelado.

## Correos
La web ya deja preparado el flujo para `/api/appointments` y `/api/community`.

Para producción hay que conectar un proveedor de correo de Alquimia y verificar el dominio remitente. No se utilizará el dominio de MUSA.

## Comunidad Alquimia
La sección incluye:
- nombre;
- correo;
- mascota opcional;
- consentimiento;
- mensaje de bienvenida.

El endpoint `/api/community` queda reservado para incorporar el alta automática y el correo de bienvenida.

## Empresas Amigas
Se agregó una sección propia para:
- hoteles/hospedajes pet friendly;
- servicios para mascotas;
- emprendimientos aliados;
- formulario/contacto para solicitar incorporación.

## Importante
La versión actual es la capa visual y de experiencia. No se debe considerar activo el bloqueo global de turnos ni el envío automático de correos hasta conectar el backend, calendario y dominio de correo correspondientes.
