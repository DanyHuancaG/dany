<!--
Sync Impact Report
==================
Version change: (plantilla sin versionar) → 1.0.0
Bump rationale: Primera ratificación; se reemplazan todos los placeholders de la plantilla.

Modified principles (placeholder → título definitivo):
  - [PRINCIPLE_1_NAME] → I. Catálogo Cerrado de Canchas
  - [PRINCIPLE_2_NAME] → II. Prevención de Colisiones (NO NEGOCIABLE)
  - [PRINCIPLE_3_NAME] → III. Autenticación Obligatoria
  - [PRINCIPLE_4_NAME] → IV. Simplicidad y Estructura Plana
  - [PRINCIPLE_5_NAME] → V. Estilo Funcional y Nomenclatura

Added principles:
  - VI. Manejo de Errores Seguro
  - VII. Cero Código Sombra

Added sections:
  - Naturaleza del Proyecto y Stack Tecnológico (antes [SECTION_2_NAME])
  - Flujo de Desarrollo Guiado por Especificaciones (antes [SECTION_3_NAME])
  - Governance: prioridad de documentos, enmiendas, versionado, cumplimiento

Removed sections: ninguna

Templates requiring updates (no modificados por este comando; leen la constitución en runtime):
  ⚠ .specify/templates/plan-template.md — su "Constitution Check" debe validarse contra I–VII
  ⚠ .specify/templates/spec-template.md — sin cambios requeridos detectados
  ⚠ .specify/templates/tasks-template.md — sin cambios requeridos detectados

Follow-up TODOs: ninguno.
-->

# Constitución del Sistema de Reservas de Pádel

## Core Principles

### I. Catálogo Cerrado de Canchas

El sistema MUST manejar exclusivamente las siguientes 5 canchas:

- Cancha Laureles
- Cancha El Poblado
- Cancha Belén
- Cancha Robledo
- Cancha Envigado

El sistema MUST NOT permitir crear, renombrar ni eliminar canchas fuera de este catálogo. Cualquier reserva
que referencie una cancha inexistente MUST rechazarse.

**Rationale**: El dominio está acotado a un conjunto fijo de instalaciones; un catálogo cerrado
elimina ambigüedad y evita funcionalidad administrativa no solicitada.

### II. Prevención de Colisiones (NO NEGOCIABLE)

- Bajo ninguna circunstancia se MUST registrar una reserva en la base de datos sin validar
  previamente que la cancha seleccionada esté disponible durante el horario solicitado.
- Un intento de reservar una cancha ocupada MUST responder con `409 Conflict`.
- Los horarios de reserva MUST expresarse en formato de 24 horas.

**Rationale**: La doble reserva (*double booking*) es el fallo crítico del dominio; la integridad
de la disponibilidad es la razón de ser del sistema.

### III. Autenticación Obligatoria

Todo flujo de creación o gestión de reservas MUST requerir un usuario con sesión activa. Las
peticiones sin sesión válida MUST responder con `401 Unauthorized`.

**Rationale**: Cada reserva debe estar asociada a un usuario identificable.

### IV. Simplicidad y Estructura Plana

- Se MUST evitar la sobreingeniería. Se MUST NOT implementar Clean Architecture ni patrones
  arquitectónicos complejos salvo necesidad explícitamente definida en `spec.md`.
- La estructura base del proyecto MUST ser:
  - `/frontend`
  - `/backend`
  - `/db`
- El acceso a datos MUST realizarse con `better-sqlite3` o SQL puro; los ORMs pesados están
  prohibidos.

**Rationale**: El alcance del producto es pequeño; la complejidad adicional no aporta valor y
dificulta la trazabilidad con las especificaciones.

### V. Estilo Funcional y Nomenclatura

- Se MUST priorizar la programación funcional.
- React MUST usar componentes funcionales y Hooks; los componentes basados en clases están
  prohibidos.
- Las clases MUST evitarse salvo que una dependencia o requisito técnico específico las haga
  necesarias, y dicha necesidad MUST justificarse en `plan.md`.
- Nomenclatura:
  - `camelCase` para variables y funciones.
  - `PascalCase` para interfaces, tipos y componentes de React.

**Rationale**: Un estilo uniforme y funcional reduce estado mutable oculto y facilita la revisión.

### VI. Manejo de Errores Seguro

**UI**:

- La interfaz MUST NOT exponer errores técnicos, excepciones internas ni *stack traces* al usuario
  final.
- Los errores MUST transformarse en mensajes claros y comprensibles. Ejemplo:
  "La cancha ya fue reservada en este horario."

**Backend**:

- El backend MUST usar códigos de estado HTTP semánticos:
  - `400 Bad Request`: petición inválida o datos incorrectos.
  - `401 Unauthorized`: usuario no autenticado.
  - `404 Not Found`: recurso inexistente.
  - `409 Conflict`: conflicto de reserva (cancha ocupada).
  - `500 Internal Server Error`: error interno inesperado.
- Las respuestas MUST NOT exponer información sensible sobre la implementación interna.

**Rationale**: Protege al usuario de mensajes confusos y al sistema de filtraciones de detalles
internos, manteniendo un contrato de API predecible.

### VII. Cero Código Sombra

El agente MUST construir estrictamente lo documentado en `spec.md` y MUST NOT añadir
funcionalidades no solicitadas explícitamente. En particular, no se implementarán salvo que
aparezcan explícitamente en una especificación:

- Pasarelas de pago.
- Perfiles avanzados.
- Roles adicionales.
- Notificaciones.
- Recuperación de contraseña.
- Integraciones externas.
- Reportes.
- Paneles administrativos.

**Rationale**: Cada línea de código debe ser trazable a un requisito documentado; el código sombra
introduce alcance, riesgo y mantenimiento no acordados.

## Naturaleza del Proyecto y Stack Tecnológico

**Propósito**: Aplicación para la reserva de canchas de pádel que permite a los usuarios
autenticarse y gestionar reservas de horarios en canchas específicas.

**Stack obligatorio**:

- **Frontend / UI**: React con Tailwind CSS para estilos.
- **Backend**: Node.js con Express.
- **Base de datos**: SQLite local, archivo `padel.db`.
- **Acceso a datos**: `better-sqlite3` o sentencias SQL puras (sin ORMs pesados).
- **Lenguaje**: TypeScript en todo el stack.

Cualquier desviación de este stack requiere una enmienda constitucional.

## Flujo de Desarrollo Guiado por Especificaciones

**Fuente de la verdad**: Esta Constitución establece las reglas globales y restricciones
permanentes. Las funcionalidades concretas MUST estar definidas en `spec.md`.

**Contradicciones**: Si una especificación contradice esta Constitución, el agente MUST:

1. Detener la implementación relacionada.
2. Identificar claramente la contradicción.
3. Informar al usuario.
4. Solicitar que se modifique la Constitución o la especificación antes de continuar.

El agente MUST NOT resolver silenciosamente una contradicción modificando el comportamiento
esperado.

**No asumir requisitos**: Cuando una funcionalidad, comportamiento o regla de negocio no esté
definida en `spec.md`, el agente MUST NOT inventarla ni implementarla basándose en supuestos, y
MUST limitarse al alcance documentado.

## Governance

**Prioridad de documentos** (de mayor a menor):

1. Constitución del proyecto
2. Especificaciones (`spec.md`)
3. Plan de implementación (`plan.md`)
4. Tareas (`tasks.md`)
5. Código fuente

Ningún documento de nivel inferior puede contradecir uno de nivel superior.

**Enmiendas**: Toda modificación de esta Constitución MUST realizarse mediante
`/speckit-constitution`, documentarse en el Sync Impact Report y actualizar la versión y la fecha
de última enmienda.

**Versionado** (semántico):

- MAJOR: eliminación o redefinición incompatible de principios o reglas de gobierno.
- MINOR: nuevo principio o sección, o ampliación material de una guía existente.
- PATCH: aclaraciones, redacción o correcciones sin cambio semántico.

**Cumplimiento**: Cada `plan.md` MUST incluir un "Constitution Check" contra los principios I–VII.
Toda revisión de código MUST verificar el cumplimiento, y cualquier complejidad o uso de clases
MUST quedar justificado por escrito.

**Version**: 1.0.0 | **Ratified**: 2026-10-06 | **Last Amended**: 2026-10-06
