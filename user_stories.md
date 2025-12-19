# Historias de Usuario: Turnero Médico

Formato: **Como [Rol], quiero [Acción], para [Beneficio]**.

## Épica 1: Gestión de Turnos (Pacientes)

### HU-01: Ver disponibilidad
**Como** paciente potencial,
**Quiero** ver los horarios disponibles de un médico en una fecha específica,
**Para** elegir el que mejor se adapte a mi agenda sin tener que llamar.

**Criterios de Aceptación:**
- [ ] Debe mostrar un calendario mensual para seleccionar fecha.
- [ ] Al seleccionar un día, debe mostrar burbujas con las horas "Libres".
- [ ] NO debe mostrar horarios pasados ni ocupados.

### HU-02: Reservar un turno
**Como** paciente,
**Quiero** confirmar la reserva de un horario seleccionado ingresando mis datos,
**Para** asegurarme de que el médico me atenderá.

**Criterios de Aceptación:**
- [ ] Debe pedir Nombre, Email y Teléfono (si no estoy logueado).
- [ ] Al confirmar, el horario debe desaparecer inmediatamente para otros usuarios.
- [ ] Debo recibir un email con los detalles de la cita.

### HU-03: Cancelar mi turno
**Como** paciente responsable,
**Quiero** cancelar un turno que no podré usar,
**Para** liberar el espacio para otra persona y avisar al médico.

**Criterios de Aceptación:**
- [ ] Solo puedo cancelar si faltan más de 2 horas.
- [ ] El sistema debe mostrar un mensaje de confirmación antes de borrarlo.

---

## Épica 2: Gestión de Agenda (Médicos)

### HU-04: Configurar horario laboral
**Como** médico,
**Quiero** definir qué días y horas atiendo (ej: Lunes de 9 a 13),
**Para** que el sistema genere los turnos automáticamente.

### HU-05: Bloqueo de emergencia (Vacaciones/Enfermedad)
**Como** médico,
**Quiero** bloquear un rango de fechas en mi calendario,
**Para** evitar que reserven cuando no voy a estar y cancelar los que ya estaban.

**Criterios de Aceptación:**
- [ ] Si bloqueo un día con turnos ya reservados, el sistema debe avisarme cuántos pacientes serán afectados.
- [ ] Al confirmar, debe disparar el proceso de notificación/reprogramación automática.

---

## Épica 3: Backoffice / Admin

### HU-06: Ver agenda del día
**Como** secretaria/médico,
**Quiero** ver una lista de todos los pacientes del día,
**Para** saber quién viene y marcar su asistencia (“Llegó”, “Atendido”).
