# Esquema de Base de Datos (ERD): Turnero Médico

Este diseño traduce nuestro modelo de dominio a tablas relacionales SQL.

## Diagrama Entidad-Relación

```mermaid
erDiagram
    MEDICOS ||--|{ DISPONIBILIDAD_SEMANAL : define
    MEDICOS ||--|{ BLOQUEOS_AGENDA : tiene
    MEDICOS ||--o{ TURNOS : atiende
    PACIENTES ||--o{ TURNOS : solicita
    ESPECIALIDADES ||--|{ MEDICOS : agrupa

    ESPECIALIDADES {
        int id PK
        string nombre "Ej: Cardiología"
    }

    MEDICOS {
        int id PK
        int especialidad_id FK
        string nombre
        string matricula
        string email
        string telefono
    }

    PACIENTES {
        int id PK
        string dni_cedula UK "Identificador único"
        string nombre
        string email
        string telefono
        date fecha_nacimiento
    }

    DISPONIBILIDAD_SEMANAL {
        int id PK
        int medico_id FK
        int dia_semana "0=Domingo, 1=Lunes..."
        time hora_inicio "Ej: 09:00"
        time hora_fin "Ej: 13:00"
        int duracion_turno_min "Ej: 30"
    }

    BLOQUEOS_AGENDA {
        int id PK
        int medico_id FK
        datetime fecha_inicio
        datetime fecha_fin
        string motivo "Ej: Congreso, Vacaciones"
    }

    TURNOS {
        int id PK
        int medico_id FK
        int paciente_id FK
        datetime fecha_hora
        string estado "PENDIENTE, CONFIRMADO, CANCELADO, REALIZADO"
        string notas_medicas "Opcional"
        datetime fecha_creacion
    }
```

## Detalles de Implementación SQL

### Notas de Diseño
1.  **Tabla `DISPONIBILIDAD_SEMANAL`**: Es la clave de la eficiencia. En lugar de generar millones de filas de "huecos libres", guardamos solo las reglas.
    *   *Ejemplo*: Si el Dr. atiende Lunes de 9 a 17, es 1 sola fila en esta tabla.
    *   *Ahorro*: Para 1 año, nos ahorramos generar ~5000 registros vacíos por médico.

2.  **Tabla `TURNOS`**: Solo existe si hay una reserva real.
    *   **Índice Único**: `(medico_id, fecha_hora)` debe ser UNIQUE (excepto si el estado es CANCELADO) para evitar doble reserva a nivel de base de datos.

3.  **Auditoría**: `fecha_creacion` en Turnos nos sirve para saber cuándo reservó el paciente (útil para la regla de cancelación de "2 horas antes").
