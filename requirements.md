# Requerimientos del Sistema: Turnero Médico

## 1. Requerimientos Funcionales (Lo que hace el sistema)

### Gestión de Turnos (Core)
**RF-01: Visualización de Disponibilidad**
- El sistema debe mostrar los horarios disponibles del médico agrupados por día.
- **Regla:** Solo se muestran horarios futuros y que no estén ya en estado `Reservado` o `Confirmado`.

**RF-02: Reserva de Turno (Paciente)**
- El paciente selecciona un horario disponible y completa sus datos básicos.
- El turno pasa a estado `Pendiente` o `Confirmado` (dependiendo de la configuración).

**RF-03: Cancelación por Paciente**
- **Regla de Tiempo:** Un paciente puede cancelar su turno hasta **2 horas antes** del horario programado.
- **Política de Penalización:** No se aplica ninguna multa ni penalización económica.
- **Acción del Sistema:** El estado cambia a `Cancelado` y el horario se libera inmediatamente.

**RF-04: Ausencia/Emergencia del Médico (Reprogramación Inteligente)**
- **Acción:** Si el médico bloquea su agenda, el sistema identifica los turnos afectados.
- **Lógica:** El sistema busca automáticamente el **próximo hueco disponible** y mueve el turno a estado `Pendiente de Reconfirmación`.
- **Notificación:** El paciente debe aceptar o cambiar la nueva propuesta.

---

## 2. Matriz de Roles y Permisos (Seguridad)

| Acción | Paciente (Invitado) | Paciente (Logueado) | Médico | Admin |
| :--- | :---: | :---: | :---: | :---: |
| Ver Disponibilidad | ✅ | ✅ | ✅ | ✅ |
| Reservar Turno | ✅ (Requiere Validar Email) | ✅ | ❌ | ❌ |
| Cancelar Turno Propio | ❌ | ✅ | ❌ | ✅ |
| Cancelar Cualquier Turno | ❌ | ❌ | ✅ | ✅ |
| Bloquear Agenda | ❌ | ❌ | ✅ | ✅ |
| Ver Estadísticas | ❌ | ❌ | ✅ | ✅ |

---

## 3. Requerimientos No Funcionales (Calidad)

### RNF-01: Seguridad
- **Autenticación:** Uso de JSON Web Tokens (JWT).
- **Contraseñas:** Almacenamiento hasheado (Bcrypt o Argon2).
- **Datos Sensibles:** La historia clínica (si se implementara en el futuro) debe estar encriptada en reposo.

### RNF-02: Rendimiento (Performance)
- **Tiempo de Respuesta:** La búsqueda de disponibilidad debe tardar menos de **500ms** incluso con agenda llena.
- **Concurrencia:** El sistema debe manejar bloqueos optimistas para evitar que dos pacientes reserven el mismo turno exacta y simultáneamente.

### RNF-03: Disponibilidad
- El sistema debe estar operativo 24/7 (SaaS en la nube).
- Backups automáticos diarios de la base de datos.
