# Sistema MediSalud — Diagramas para GitDiagram / Mermaid

Este archivo contiene los diagramas del informe **Sistema MediSalud** convertidos a sintaxis Mermaid para poder copiarlos en herramientas compatibles con Mermaid/GitDiagram.

---

## 1. Diagrama de clases del dominio

```mermaid
classDiagram

class Usuario {
  +int idUsuario
  +string nombreUsuario
  +string contrasena
  +string rol
  +string estado
}

class Paciente {
  +int idPaciente
  +string DNI
  +string nombres
  +string apellidos
  +date fechaNacimiento
  +string telefono
  +string direccion
  +string seguro
}

class Medico {
  +int idMedico
  +string nombres
  +string apellidos
  +string CMP
  +string telefono
}

class Especialidad {
  +int idEspecialidad
  +string nombre
  +string descripcion
}

class Consultorio {
  +int idConsultorio
  +string nombre
  +string ubicacion
}

class Cita {
  +int idCita
  +date fecha
  +time hora
  +string estado
  +string motivo
}

class HistoriaClinica {
  +int idHistoria
  +date fechaCreacion
  +string antecedentes
  +string alergias
  +string observaciones
}

class Consulta {
  +int idConsulta
  +date fecha
  +string sintomas
  +string diagnostico
  +string observaciones
}

class Pago {
  +int idPago
  +date fecha
  +decimal monto
  +string metodoPago
  +string estado
}

class Receta {
  +int idReceta
  +date fecha
  +string indicaciones
}

class Tratamiento {
  +int idTratamiento
  +string descripcion
  +string duracion
  +string indicaciones
}

class Medicamento {
  +int idMedicamento
  +string nombre
  +string presentacion
  +int stock
}

Usuario "1" --> "0..1" Paciente : es
Usuario "1" --> "0..1" Medico : es
Paciente "1" --> "1" HistoriaClinica : posee
Paciente "1" --> "0..*" Cita : solicita
Medico "1" --> "0..*" Cita : atiende
Medico "0..*" --> "1" Especialidad : pertenece a
Medico "0..*" --> "0..*" Consultorio : atiende en
Cita "1" --> "0..1" Consulta : genera
Cita "1" --> "0..1" Pago : genera
Consulta "1" --> "0..*" Receta : genera
Consulta "1" --> "0..*" Tratamiento : indica
Receta "1" --> "1..*" Medicamento : contiene
HistoriaClinica "1" --> "0..*" Consulta : contiene
```

---

## 2. Diagrama de casos de uso

```mermaid
flowchart LR

P[Paciente]
M[Médico]
R[Recepcionista]
A[Administrador]
F[Farmacia]

subgraph SCP["Casos de uso - Paciente"]
  UC1((Solicitar cita))
  UC2((Consultar citas))
  UC3((Ver historia clínica))
  UC4((Ver receta médica))
end

subgraph SCM["Casos de uso - Médico"]
  UC5((Consultar agenda))
  UC6((Atender consulta))
  UC7((Registrar diagnóstico))
  UC8((Generar receta))
end

subgraph SCR["Casos de uso - Recepcionista"]
  UC9((Registrar paciente))
  UC10((Programar cita))
  UC11((Reprogramar / cancelar cita))
  UC12((Registrar pago))
end

subgraph SCA["Casos de uso - Administrador"]
  UC13((Gestionar médicos))
  UC14((Gestionar especialidades))
  UC15((Gestionar usuarios))
  UC16((Generar reportes))
end

subgraph SCF["Casos de uso - Farmacia"]
  UC17((Gestionar medicamentos))
  UC18((Despachar receta))
  UC19((Controlar stock))
end

P --- UC1
P --- UC2
P --- UC3
P --- UC4

M --- UC5
M --- UC6
UC6 -. include .-> UC7
UC7 -. extend .-> UC8

R --- UC9
R --- UC10
R --- UC11
R --- UC12

A --- UC13
A --- UC14
A --- UC15
A --- UC16

F --- UC17
F --- UC18
UC18 -. include .-> UC19
F --- UC19
```

---

## 3. Diagrama de estados de una cita

```mermaid
stateDiagram-v2

[*] --> Programada

Programada --> Confirmada : confirmar
Programada --> Reprogramada : reprogramar
Programada --> Cancelada : cancelar

Reprogramada --> Confirmada : confirmar
Reprogramada --> Cancelada : cancelar

Confirmada --> Atendida : atender
Confirmada --> Cancelada : cancelar

Atendida --> [*]
Cancelada --> [*]
```

---

## 4. Diagrama de flujo del proceso de agendamiento

```mermaid
flowchart TD

INI([Inicio])
SOL["Paciente solicita<br/>una cita médica"]
BUS["Recepcionista busca<br/>especialidad y médico"]
DEC1{"¿Existe horario<br/>disponible?"}
REG["Registrar la cita<br/>(estado: Programada)"]
ALT["Ofrecer horarios<br/>alternativos"]
CONF["Enviar confirmación<br/>al paciente"]
DEC2{"¿Paciente confirma<br/>asistencia?"}
OK["Cita queda<br/>Confirmada"]
CANCEL["Cita queda<br/>Cancelada"]
FIN([Fin])

INI --> SOL
SOL --> BUS
BUS --> DEC1

DEC1 -->|Sí| REG
DEC1 -->|No| ALT
ALT --> BUS

REG --> CONF
CONF --> DEC2

DEC2 -->|Sí| OK
DEC2 -->|No| CANCEL

OK --> FIN
CANCEL --> FIN
```

---

## 5. Diagrama de arquitectura del sistema

```mermaid
flowchart TD

SEC["Seguridad y autenticación<br/>(control de acceso por rol)"]

subgraph PRESENTACION["Capa de Presentación"]
  PM["Panel médico<br/>(consultas)"]
  MR["Módulo de<br/>recepción / admisión"]
  PW["Portal web<br/>(pacientes)"]
end

subgraph NEGOCIO["Capa de Lógica de Negocio"]
  GP["Gestión de pagos<br/>y reportes"]
  GM["Gestión de<br/>medicamentos y recetas"]
  GH["Gestión de<br/>historias clínicas"]
  GC["Gestión de citas"]
end

subgraph DATOS["Capa de Acceso a Datos"]
  DAO["Repositorios / DAO<br/>(ORM)"]
end

subgraph BD["Capa de Base de Datos"]
  DB["Base de datos<br/>relacional"]
end

SEC -.-> PM
SEC -.-> MR
SEC -.-> PW

PM --> GH
MR --> GC
PW --> GC

GP --> DAO
GM --> DAO
GH --> DAO
GC --> DAO

DAO --> DB
```

---

## 6. Diagrama ER simplificado para base de datos

Si GitDiagram se está utilizando específicamente para un **diagrama entidad-relación**, se puede utilizar esta versión:

```mermaid
erDiagram

USUARIO {
    int idUsuario PK
    string nombreUsuario
    string contrasena
    string rol
    string estado
}

PACIENTE {
    int idPaciente PK
    string DNI
    string nombres
    string apellidos
    date fechaNacimiento
    string telefono
    string direccion
    string seguro
}

MEDICO {
    int idMedico PK
    string nombres
    string apellidos
    string CMP
    string telefono
    int idEspecialidad FK
}

ESPECIALIDAD {
    int idEspecialidad PK
    string nombre
    string descripcion
}

CONSULTORIO {
    int idConsultorio PK
    string nombre
    string ubicacion
}

CITA {
    int idCita PK
    date fecha
    string hora
    string estado
    string motivo
    int idPaciente FK
    int idMedico FK
}

HISTORIA_CLINICA {
    int idHistoria PK
    date fechaCreacion
    string antecedentes
    string alergias
    string observaciones
    int idPaciente FK
}

CONSULTA {
    int idConsulta PK
    date fecha
    string sintomas
    string diagnostico
    string observaciones
    int idCita FK
}

PAGO {
    int idPago PK
    date fecha
    decimal monto
    string metodoPago
    string estado
    int idCita FK
}

RECETA {
    int idReceta PK
    date fecha
    string indicaciones
    int idConsulta FK
}

TRATAMIENTO {
    int idTratamiento PK
    string descripcion
    string duracion
    string indicaciones
    int idConsulta FK
}

MEDICAMENTO {
    int idMedicamento PK
    string nombre
    string presentacion
    int stock
}

RECETA_MEDICAMENTO {
    int idReceta FK
    int idMedicamento FK
}

USUARIO ||--o| PACIENTE : "es"
USUARIO ||--o| MEDICO : "es"

ESPECIALIDAD ||--o{ MEDICO : "tiene"
PACIENTE ||--|| HISTORIA_CLINICA : "posee"

PACIENTE ||--o{ CITA : "solicita"
MEDICO ||--o{ CITA : "atiende"

CITA ||--o| CONSULTA : "genera"
CITA ||--o| PAGO : "genera"

CONSULTA ||--o{ RECETA : "genera"
CONSULTA ||--o{ TRATAMIENTO : "indica"

RECETA ||--o{ RECETA_MEDICAMENTO : "contiene"
MEDICAMENTO ||--o{ RECETA_MEDICAMENTO : "aparece en"

HISTORIA_CLINICA ||--o{ CONSULTA : "contiene"
```

---

## 7. Diagrama de secuencia — Programar cita médica

```mermaid
sequenceDiagram

actor Paciente
actor Recepcionista
participant Sistema
participant Medico

Paciente->>Recepcionista: Solicita cita
Recepcionista->>Sistema: Selecciona especialidad
Sistema->>Sistema: Busca médicos disponibles
Sistema-->>Recepcionista: Muestra médicos disponibles
Recepcionista->>Sistema: Selecciona médico y horario
Sistema->>Medico: Consulta disponibilidad
Medico-->>Sistema: Confirma disponibilidad
Sistema->>Sistema: Registra cita como Programada
Sistema-->>Paciente: Envía confirmación
Sistema-->>Medico: Actualiza agenda
```

---

## Nota

Los diagramas fueron adaptados del informe de **MediSalud** a sintaxis Mermaid. El contenido textual del informe se mantiene como referencia, mientras que los diagramas fueron expresados mediante código para que puedan editarse y generarse nuevamente en herramientas compatibles.
