# Sistema MediSalud

## Sistema de Gestión de Servicios de Salud

MediSalud es un sistema de información para una empresa privada de servicios de salud. Su objetivo es centralizar la gestión de pacientes, médicos, especialidades, citas, historias clínicas, consultas, tratamientos, recetas, medicamentos y pagos.

## Problema

Actualmente el registro de pacientes, la programación de citas y el manejo de historias clínicas pueden realizarse de forma manual o mediante hojas de cálculo independientes. Esto puede generar duplicidad de información, demoras en la atención y dificultades para consultar el historial médico.

## Objetivo

Diseñar un sistema que permita centralizar y organizar la información de la atención ambulatoria, facilitando la gestión de citas, consultas, historias clínicas, recetas, medicamentos y pagos.

## Módulos principales

- Usuarios y control de acceso
- Pacientes
- Médicos
- Especialidades
- Consultorios
- Citas
- Historias clínicas
- Consultas médicas
- Tratamientos
- Recetas
- Medicamentos
- Pagos
- Reportes

## Entidades principales

| Entidad | Función |
|---|---|
| Usuario | Cuenta de acceso y rol del sistema |
| Paciente | Datos de la persona atendida |
| Médico | Información del profesional de salud |
| Especialidad | Área médica del profesional |
| Consultorio | Lugar de atención |
| Cita | Reserva de atención |
| Historia Clínica | Historial médico del paciente |
| Consulta | Atención médica realizada |
| Tratamiento | Indicaciones terapéuticas |
| Receta | Indicaciones y medicamentos prescritos |
| Medicamento | Catálogo y control de medicamentos |
| Pago | Registro del pago asociado a una cita |

## Documentación y diagramas

Los diagramas editables en Mermaid se encuentran en la carpeta `docs/`.

- [Modelo de dominio](docs/modelo-dominio.md)
- [Casos de uso](docs/casos-uso.md)
- [Diagrama ER](docs/diagrama-er.md)
- [Diagrama de clases](docs/diagrama-clases.md)
- [Diagrama de secuencia](docs/diagrama-secuencia.md)
- [Diagrama de estados](docs/diagrama-estados.md)
- [Flujo de agendamiento](docs/flujo-agendamiento.md)
- [Arquitectura](docs/arquitectura.md)

También se incluye `MediSalud_GitDiagram.md` con todos los diagramas reunidos en un solo archivo.

## Reglas de negocio principales

1. Cada paciente debe tener un identificador único.
2. Cada médico debe estar asociado a una especialidad.
3. Una cita relaciona a un paciente con un médico.
4. Una cita puede generar una consulta.
5. Una consulta puede generar recetas y tratamientos.
6. Una receta puede contener uno o varios medicamentos.
7. Una cita puede generar un pago.
8. El acceso al sistema se controla mediante usuarios y roles.

## Documentos originales

Este repositorio conserva los documentos utilizados como base para el modelado del sistema.
