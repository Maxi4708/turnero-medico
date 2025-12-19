# Diseño de API REST: Turnero Médico

Interfaz de comunicación entre el Frontend (React/Web) y el Backend.

## 1. Módulo Público / Pacientes

### Buscar Disponibilidad
Calcula los horarios libres basándose en disponibilidad base - bloqueos - turnos ocupados.
- **GET** `/api/v1/availability`
- **Query Params**: `doctor_id`, `date_from`, `date_to`
- **Response**:
```json
[
  {
    "date": "2023-10-10",
    "slots": ["09:00", "09:30", "11:00"] // 10:00 y 10:30 estaban ocupados
  }
]
```

### Reservar Turno
- **POST** `/api/v1/appointments`
- **Body**:
```json
{
  "doctor_id": 1,
  "start_time": "2023-10-10T09:00:00Z"
  // El backend infiere el paciente del token de sesión o pide datos si es público
}
```

### Mis Turnos (Paciente)
- **GET** `/api/v1/patients/me/appointments`
- **Response**: Lista de turnos con estado.

### Cancelar Turno
- **POST** `/api/v1/appointments/{id}/cancel`
- **Regla**: Valida que falten > 2 horas (RF-03).

## 2. Módulo Médico

### Configurar Agenda Base
- **POST** `/api/v1/doctors/me/availability`
- **Body**:
```json
{
  "day_of_week": 1, // Lunes
  "start": "09:00",
  "end": "13:00",
  "slot_duration": 30
}
```

### Bloquear Agenda (Urgencia/Vacaciones)
- **POST** `/api/v1/doctors/me/blocks`
- **Body**:
```json
{
  "start_date": "2023-12-01",
  "end_date": "2023-12-15",
  "reason": "Vacaciones"
}
```
- **Efecto Secundario (Backend)**: Dispara el proceso de "Reprogramación Inteligente" (RF-04) para los turnos que caen en este rango.

## 3. Códigos de Error Comunes
- `409 Conflict`: "El turno ya fue reservado por otro paciente hace un instante".
- `422 Unprocessable Entity`: "No se puede cancelar con menos de 2 horas de antelación".
