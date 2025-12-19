# Visión y Alcance: Sistema de Gestión de Turnos Médicos ("Turnero")

## 1. Problema de Negocio
La interacción actual entre pacientes y médicos es ineficiente. Los pacientes deben llamar durante horario de oficina para reservar, lo que genera pérdida de tiempo y oportunidades perdidas. Los médicos tienen dificultades para gestionar sus agendas y manejar las cancelaciones de manera efectiva.

## 2. Visión del Producto
Para **consultorios médicos pequeños y doctores independientes** que necesitan **una forma eficiente de organizar su agenda**, el **Turnero** es un sistema de gestión web que ofrece **reservas de autoservicio 24/7 para pacientes y gestión de calendario para doctores**. A diferencia de usar un simple Google Calendar o una agenda de papel, este producto **hace cumplir reglas de negocio (duración, prevención de conflictos) y gestiona los estados de los turnos automáticamente**.

## 3. Alcance (MVP - Producto Mínimo Viable)

### Dentro del Alcance
- **Portal Público**: Los pacientes pueden ver la disponibilidad de los médicos y reservar turnos.
- **Portal del Médico**: Los médicos pueden configurar sus horarios de atención y franjas horarias.
- **Ciclo de Vida del Turno**: Seguimiento de estados (Pendiente, Confirmado, Cancelado, Completado).
- **Notificaciones**: Confirmaciones por correo electrónico (simulado por ahora).

### Fuera del Alcance (por ahora)
- Pagos en línea.
- Historia Clínica Electrónica (HCE).
- Sistema SaaS Multi-inquilino (construiremos para un solo consultorio primero).

## 4. Actores Clave
- **Paciente**: Persona que busca atención médica.
- **Médico**: Profesional médico que presta el servicio.
- **Secretaria/Admin**: Rol opcional para gestionar los datos maestros del consultorio.
