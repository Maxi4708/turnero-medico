# Diagramas del Sistema: Turnero Médico

## 1. Diagrama de Flujo: Cálculo de Disponibilidad
Este diagrama explica la lógica que usa el sistema para responder a la pregunta: *"¿Qué horarios tiene libres el Dr. X el día Y?"*

```mermaid
flowchart TD
    Start(["Inicio: Petición de Horarios"]) --> CheckBase{"¿Tiene Agenda Base ese día?"}
    CheckBase -- NO --> ReturnEmpty["Retornar: Sin Disponibilidad"]
    CheckBase -- SI --> GenSlots["Generar Slots en memoria<br/>(ej: 9:00, 9:30, 10:00...)"]
    
    GenSlots --> CheckBlock{"¿Hay Bloqueo (Vacaciones)?"}
    CheckBlock -- SI --> FilterBlock["Eliminar slots dentro del Bloqueo"]
    CheckBlock -- NO --> CheckAppts
    FilterBlock --> CheckAppts
    
    CheckAppts{"¿Hay Turnos ya reservados?"}
    CheckAppts -- SI --> FilterAppts["Eliminar slots ocupados"]
    CheckAppts -- NO --> FinalList
    FilterAppts --> FinalList
    
    FinalList(["Fin: Retornar Lista de Horarios Libres"])
```

## 2. Diagrama de Secuencia: Reserva de Turno (Happy Path)
Interacción cuando un Paciente reserva con éxito.

```mermaid
sequenceDiagram
    actor Paciente
    participant UI as Frontend
    participant API as Backend API
    participant DB as Base de Datos
    participant Email as Servicio Email

    Paciente->>UI: Selecciona Médico y Fecha
    UI->>API: GET /availability
    API->>DB: Consulta Config + Turnos
    DB-->>API: Retorna datos crudos
    API->>API: Calcula libres (ver Flujo arriba)
    API-->>UI: Lista de Horarios [9:00, 9:30...]
    
    Paciente->>UI: Elige 09:30 y Confirma
    UI->>API: POST /appointments (Dr, Fecha, Paciente)
    
    rect rgb(200, 255, 200)
    note right of API: Transacción Atómica
    API->>DB: Verificar Disponibilidad (doble chequeo)
    DB-->>API: OK (Libre)
    API->>DB: INSERT INTO Turnos
    DB-->>API: ID Turno Creado
    end
    
    par Notificaciones Async
        API->>Email: Enviar Confirmación Paciente
        API->>Email: Notificar Médico
    end
    
    API-->>UI: 201 Created
    UI-->>Paciente: Muestra "¡Turno Confirmado!"
```

## 3. Diagrama de Secuencia: Cancelación Masiva (Caso Complejo)
Qué pasa cuando el Médico bloquea su agenda (RF-04).

```mermaid
sequenceDiagram
    actor Medico
    participant API as Backend API
    participant DB as Base de Datos
    participant Bot as Worker Reprogramación

    Medico->>API: POST /blocks (Vacaciones del 10 al 15)
    API->>DB: INSERT INTO Bloqueos
    API->>Bot: Disparar Evento "AgendaBloqueada"
    API-->>Medico: 200 OK (Bloqueo creado)

    loop Para cada turno afectado
        Bot->>DB: SELECT turnos en rango
        DB-->>Bot: Lista de Turnos [T1, T2...]
        
        bot->>DB: UPDATE Turno SET estado = 'PENDIENTE_REPROG'
        
        Bot->>Bot: Buscar próximo hueco libre (Algoritmo)
        Bot->>DB: Reservar Slot Tentativo
        
        Bot->>Medico: Notificar (Email/SMS) al Paciente
        note right of Bot: "Su turno fue movido. Confirme por favor."
    end
```
