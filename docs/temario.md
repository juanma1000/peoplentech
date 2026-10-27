# Estructurar el desarrollo de software con IA, mediante harnesses y SDD

**Duración:** 30 min

**Objetivo:** Mostrar cómo estructurar el ciclo de desarrollo de software con agentes de IA, desde el Discovery hasta el Delivery, mediante harnesses que guían y controlan el trabajo de los agentes y un enfoque SDD que convierte las especificaciones en software verificable.

## Agenda

| # | Bloque | Tiempo |
|---|--------|--------|
| 1 | Introducción: agentes y harness | 3 min |
| 2 | Discovery: benchmarking y PRD | 4 min |
| 3 | Arquitectura | 4 min |
| 4 | Historias de usuario y estimación | 5 min |
| 5 | SDD y estándares: de la especificación al código | 9 min |
| 6 | Seguimiento: informe de avance | 2 min |
| 7 | Cierre: evolución del harness y preguntas | 3 min |

> Los tiempos por bloque son tentativos (suman 30 min); incluyen las demos en vivo.
>
> Las cuatro demos en vivo (PRD, arquitectura, historias de usuario y OpenSpec) tienen un estilo propio en la presentación: fondo oscuro, banner "Demo en vivo" y el aviso "Cambia a la terminal".

---

## 1. Introducción: agentes y harness (3 min)

### 1.1 Agente + harness
El modelo es el motor; el harness es todo lo que lo rodea, lo guía y lo controla:
- **Skills**: capacidades empaquetadas que el agente carga cuando la tarea las necesita
- **Reglas**: normas y convenciones del proyecto
- **Memoria**: contexto persistente entre sesiones
- **Verificaciones**: tests, lint y revisión antes de entregar
- **Permisos**: qué herramientas y comandos puede ejecutar el agente
- **Subagentes**: agentes especializados con rol y contexto propio

### 1.2 Ciclo de vida: Discovery → Delivery
Dos grandes bloques. Cada etapa tiene una entrada, un agente y una salida.

**Discovery** (qué construir)
- Discovery: Subagente Product Manager → PRD
- Arquitectura: Subagente Software Architect → patrón, ADR y diagramas
- HU: Subagente Product Manager → historias de usuario

**Delivery** (cómo construirlo y entregarlo)
- Specs: flujo SDD (Specify → Plan → Tasks) → spec.md, plan y tasks
- Implementación: Subagentes Backend Expert, Frontend Expert y QA Expert → código tarea a tarea
- Entrega: tests, CI y review → entrega verificada

## 2. Discovery (4 min)

### 2.1 Benchmarking con IA de investigación
Herramientas: Perplexity, NotebookLM u otras. Búsqueda en la web para obtener:
- Objetivo de aplicaciones similares
- Valor añadido y ventajas competitivas
- Funciones principales
- Resultado: documento de benchmark, insumo del PRD

### 2.2 Generación del PRD
- Qué es un PRD (Product Requirements Document): define qué se construye y por qué, antes del cómo
- Entradas: documento de benchmark (generado en el paso anterior) e idea de producto
- Secciones: objetivo, usuarios, alcance, funciones clave y criterios de éxito

### 2.3 Demo en vivo: PRD con una skill
- `/draft-prd` → responder preguntas (idea, usuarios, alcance) → PRD generado

## 3. Arquitectura (4 min)

### 3.1 Patrones de arquitectura
Los seis patrones se ven a la vez, cada uno con su diagrama y una nota (sin clics):
- Capas: presentación, negocio y datos
- Microkernel: núcleo mínimo más plugins
- Microservicios: servicios independientes y desplegables
- Hexagonal: dominio al centro, con puertos y adaptadores (la que usa `/draft-architecture`)
- Limpia: las dependencias apuntan hacia el dominio
- Eventos: componentes que se comunican por eventos

### 3.2 Skill `/draft-architecture`
Con el patrón elegido y el PRD, el Subagente Software Architect genera `docs/architecture/architecture.md`: diagramas, estructura de carpetas y decisiones (ADR).

### 3.3 Demo en vivo: arquitectura con una skill
- `/draft-architecture` → Subagente Software Architect lee el PRD y diseña → documento generado

> Cuanto mejor sea tu arquitectura, mejor será el código que la IA genera para ti.

## 4. Historias de usuario y estimación (5 min)

### 4.1 Skills de historias de usuario
Tres skills encadenadas (entra → sale):
1. `/draft-user-stories`: PRD → un .md por HU en `docs/user-stories/`
2. `/push-user-story-to-ado`: HU en archivo → User Story en Azure DevOps (verifica duplicados)
3. `/generate-ado-tasks`: ID de la HU → 3 tareas (Diseño, Implementación y Tests)

Detrás de cada HU y cada tarea hay un subagente Product Manager.

### 4.2 Estimación: puntos de historia y horas
- **Puntos de historia** (al crear la HU): escala Fibonacci (01, 02, 03, 05, 08, 13, 21); tamaño relativo entre HU, no son horas
- **Horas de las tareas** (al crear las tareas): Diseño 1h y Tests 1h fijos; Implementación de 1h a 4h según el eje de investigación
  - Patrón conocido: 1h
  - Lógica nueva: 2h
  - Esquema no verificado: 3h
  - Descubrimiento externo: 4h
- El tiempo lo marca cuánto terreno desconocido hay que investigar, no el tamaño. Resultado: de 3h a 6h por HU, en horas enteras

### 4.3 Demo en vivo: historias de usuario y tareas
- `/draft-user-stories` → HU en `docs/user-stories/HU-NNN.md` → `/push-user-story-to-ado` → `/generate-ado-tasks`

## 5. SDD y estándares (9 min)

> A partir de aquí estamos en **Delivery**: los insumos son las salidas de Discovery (PRD, arquitectura y HU).

### 5.1 SDD: Spec-Driven Development
- Las SPECS son archivos (versionados, revisables)
- Reducen alucinaciones: el agente trabaja contra un contrato
- Specs verificables: criterios de aceptación → pruebas

Comparación de enfoques (OpenSpec es el que se usa en el demo):
- **OpenSpec**: ligero, propuestas de cambio sobre proyectos existentes (`/opsx:propose` → `/opsx:apply` → `/opsx:archive`)
- **Spec Kit**: de GitHub, flujo estructurado que parte de la constitución del proyecto (`/speckit.constitution` → `.specify` → `.plan` → `.tasks` → `.implement`)
- **BMAD**: equipo ágil de agentes con roles; cubre todo el ciclo, el más pesado
- **SDD propio**: a la medida de la fábrica, con skills y subagentes propios integrados con Azure DevOps (`/specify` → `/plan` → `/tasks` → `/implement` → `/verify`)

### 5.2 Flujo: Specify → Plan → Tasks → Implement
Cuatro fases visibles a la vez, con su salida y su comando de OpenSpec:
- Specify: qué y por qué, sin cómo; escenarios y criterios de aceptación. Sale `spec.md`. Comando: `/opsx:propose`
- Plan: cómo (stack, arquitectura, contratos, ADR aplicables). Sale `design.md`. Comando: `/opsx:propose`
- Tasks: tareas pequeñas, ordenadas y comprobables. Sale `tasks.md`. Comando: `/opsx:propose`
- Implement: el agente implementa tarea a tarea y se verifica contra la spec. Salen código y tests. Comando: `/opsx:apply`
- En OpenSpec, `/opsx:propose` genera de una vez la spec, el diseño y las tareas
- Specify + Plan + Tasks son el 70% del esfuerzo; Implement, el 30%

### 5.3 Estándares del proyecto
`CLAUDE.md` es un symlink a `docs/base-standards.md`, que enlaza al resto. Los estándares se usan en Specify, Plan, Tasks e Implement.
- `base-standards.md`: principios (pasos cortos, TDD, tipado, nombres claros), idioma (código en inglés, documentación en español), skills y requisito de modelo
- `.claude/skills/`: skills del proyecto

### 5.4 Estándares derivados de base-standards
- **backend-standards**: FastAPI + DDD y SOLID, diseño de API, base de datos, testing y seguridad
- **frontend-standards**: React y microfrontends, estructura del proyecto, UI/UX, testing
- **documentation-standards**: documentación técnica, AI specs, reglas de idioma
- **openspec-tasks-mandatory-steps**: pasos obligatorios en tasks.md, tests unitarios, pruebas manuales ejecutadas por el agente

### 5.5 Demo en vivo: OpenSpec
- `/opsx:explore` (investigar y aclarar) → `/opsx:propose` (proposal, specs, tasks) → `/opsx:apply` (implementar tarea a tarea)

## 6. Seguimiento: informe de avance (2 min)

Skill `/generate-status-report`: a partir del ID de la Iniciativa en Azure DevOps, el Subagente Project Manager lee HU, tareas y bugs y genera `reports/status/estado-proyecto.md`.
- Avance real ponderado por Story Points
- Forecast de fecha: velocidad histórica y 3 escenarios (optimista, probable, conservador)
- Riesgos, bloqueos y tendencia de bugs
- Dashboard HTML

## 7. Cierre: evolución del harness (3 min)

Dos peldaños:

- **Hoy, agentes guiados**
  - Agentes con rol
  - Specs como contrato
  - Verificación: tests y estándares
- **Evolución: más escala, más producto, retroalimentación**
  - Escala y control
    - Agentes en paralelo
    - Revisor independiente
    - Memoria compartida entre proyectos
    - Harness común para toda la fábrica de software
  - Discovery de producto
    - Entrevistas con usuarios
    - Formular y validar hipótesis de producto
    - Priorización de oportunidades
    - Mapas de journey
    - Pruebas de concepto
  - Retroalimentación continua
    - Producción alimenta el Discovery
    - Auto-reparación de errores

Cierre con preguntas.
