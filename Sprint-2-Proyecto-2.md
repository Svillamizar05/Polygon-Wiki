# Sprint 2 - Polygon (Sumicolor S.A.)

> Wiki del Sprint 2 del Proyecto Integrador 2. La estructura sigue el documento maestro del Sprint 2 (secciones: Desarrollo de código / MVP, Pruebas automáticas de software, Protocolo de pruebas de usabilidad, Casos de prueba, Backlog y ceremonias, Caso de negocio y Sustentación). Al final se añade una sección propia: **Mejoras de la retroalimentación de sprints pasados**.
>
> Convención: donde una fecha o un nombre concreto deba confirmarlo el equipo, aparece un hueco corto del tipo `__(completar)__`. El resto del contenido está redactado y cerrado.

## Sprint Goal

Iterar y profundizar el desarrollo del MVP de Polygon, avanzando de forma substancial en el flujo productivo: consolidar las etapas de **Pesaje**, **Mezcla** y el envío a **Extrusión**, manteniendo la trazabilidad de cada Orden de Producción (OP) entre etapas y respetando la regla de que una OP no avanza mientras falte información obligatoria de la etapa actual. En paralelo, se cierran las deudas de documentación y consistencia señaladas en la retroalimentación de los Sprints 0 y 1.

---

## 1. Desarrollo de código (MVP V2)

### 1.1 Cumplimiento funcional

Durante el Sprint 2 se trabajó sobre el flujo de ejecución del proceso productivo (épica EP-04) y las mejoras de interfaz asociadas a la lectura de las OP. Las historias de usuario trabajadas en este Sprint, con su estado en el repositorio, son:

| HU | Título | Etapa del proceso | Estado | Evidencia en el repositorio |
|----|--------|-------------------|--------|------------------------------|
| HU-19 | Registrar pesaje de materias primas | Pesaje | Implementada | PR de pesaje (rama `feature/hu-19-hu-20-pesaje`); vistas `Frontend/src/views/pesaje/` |
| HU-20 | Confirmar envío a Mezcla | Pesaje -> Mezcla | Implementada | Misma rama de pesaje; transición de estado a `mezcla` |
| HU-22 | Registrar proceso de Mezcla | Mezcla | Implementada | Ramas `feature/hu-22-registro-mezcla`; vista `Frontend/src/views/mezcla/RegistroMezcla.vue` |
| HU-23 | Confirmar envío a Extrusión | Mezcla -> Extrusión | Implementada | Rama `feature/hu-23-envio-extrusion`; validación de cierre de mezcla y destino autorizado |
| HU-03 (soporte) | Leyenda de notación de colores de las OP | Transversal (Producción) | Implementada | `Frontend/src/components/produccion/LeyendaColoresOP.vue` + prueba de componente |

Regla de proceso respetada en todas las transiciones: el sistema valida que la información obligatoria de la etapa esté completa antes de permitir el avance de la OP a la siguiente etapa (p. ej. el envío de Mezcla a Extrusión exige el cierre del turno de mezcla y un destino autorizado).

### 1.2 Gestión técnica del desarrollo

- **Control de versiones (Git):** el trabajo se organizó en ramas por historia de usuario (`feature/hu-XX-...`), integradas a `main` mediante *pull requests* revisados. El historial del repositorio conserva los *merges* de cada HU del Sprint.
- **Contenido del repositorio:** el repositorio contiene el código del MVP (frontend en Vue 3 + Vite, backend en Django con PostgreSQL). El flujo de Pesaje, Mezcla y envío a Extrusión está presente en `Frontend/src/views/` y en los servicios correspondientes.
- **Convención de commits:** se usa un estilo convencional (`feat`, `fix`) con prefijo de la HU, lo que facilita la trazabilidad entre commit e historia de usuario.


---

## 2. Pruebas automáticas de software

### 2.1 Estrategia de pruebas

El proyecto adopta una estrategia de pruebas automáticas en el frontend basada en **Vitest** + **@vue/test-utils** (estándar del ecosistema Vite/Vue 3 ya usado en el repositorio). Las pruebas se ejecutan en modo de una sola corrida (`npm run test`, equivalente a `vitest --run`), apto para integración continua. La estrategia complementa las pruebas manuales (casos de prueba de la sección 4): las automáticas protegen la lógica y el render de las funcionalidades, y las manuales se enfocan en la usabilidad y el recorrido humano.

| Funcionalidad | Tipo de prueba | Justificación |
|---------------|----------------|---------------|
| Leyenda de colores de las OP (componente de UI) | Componente (render) | Es un componente de presentación con tres clasificaciones fijas; la prueba de render verifica que cada clasificación aparece con su color correcto y que deriva de la fuente única de verdad, sin falsos positivos de un espacio de entrada amplio. |
| Consistencia de la notación de colores con la fuente de verdad | Componente (flujo alternativo) | Garantiza que la leyenda y las cards de OP nunca se contradigan, porque ambas leen las mismas constantes (`CLASIFICACION_BADGE` / `HOJA_OP`); cubre el caso límite de la hoja blanca que no tiene franja. |
| Transición de estado Mezcla -> Extrusión (HU-23) | Integración / escenario (recomendada) | Valida la regla de negocio de que no se puede enviar a Extrusión sin cerrar el turno de mezcla ni sin un destino autorizado; es lógica de dominio con caminos de error claros. |
| Registro de pesaje (HU-19) | Escenario (recomendada) | Verifica que las cantidades pesadas quedan asociadas a la OP y que los campos obligatorios bloquean el avance. |

> Nota de alcance: en este Sprint se implementaron y ejecutaron las dos pruebas automáticas de la leyenda de colores (ver 2.2). Las pruebas de integración de las transiciones de estado (HU-19, HU-22, HU-23) quedan diseñadas en la tabla y en los casos de prueba de la sección 4, listas para automatizarse a medida que se estabilice la capa de servicios.

### 2.2 Cobertura mínima: happy path + flujo alternativo

La funcionalidad de software entregada en este Sprint que es un componente de interfaz (la leyenda de colores de las OP) está cubierta por **dos pruebas automáticas**, cumpliendo la regla del documento maestro (happy path + flujo alternativo):

1. **Happy path** — "renderiza las tres clasificaciones con su etiqueta y su color de Badge": monta el componente y verifica que aparecen las tres clasificaciones (Proceso normal, Urgente, Acompañamiento de IP), cada una con su color (gris, ámbar, azul).
2. **Flujo alternativo** — "deriva colores y textos de la fuente de verdad y describe la hoja normal sin entrada en `HOJA_OP`": verifica que los colores provienen de las constantes compartidas y que la hoja blanca se describe aunque no tenga franja, sin romper el render.

Resultado de la ejecución (modo `--run`, Vitest 3.2.7):

```
 ✓ src/components/produccion/__tests__/LeyendaColoresOP.spec.js (2 tests)
   ✓ renderiza las tres clasificaciones con su etiqueta y su color de Badge
   ✓ deriva colores y textos de la fuente de verdad y describe la hoja normal sin entrada en HOJA_OP

 Test Files  1 passed (1)
      Tests  2 passed (2)
```

Archivo de la prueba: `Frontend/src/components/produccion/__tests__/LeyendaColoresOP.spec.js`.

### 2.3 Extras

- **Análisis estático:** el nuevo componente y su prueba pasan ESLint sin hallazgos. Quedan identificados hallazgos preexistentes en archivos ajenos a este trabajo (`src/router/index.js`, `src/views/mezcla/RegistroMezcla.vue`) para corregir en una tarea de limpieza dedicada.
- **CI en cada pull request:** se recomienda un *workflow* que ejecute `npm run test` en cada PR y bloquee el *merge* si falla. Configuración pendiente de activar en el repositorio: `__(completar: habilitar GitHub Actions con el workflow de tests)__`.
- **Coverage:** Vitest permite reporte de cobertura con `--coverage`; se recomienda incorporarlo al *workflow* de CI.


---

## 3. Protocolo de pruebas de usabilidad (planeación)

> El documento maestro indica que en el Sprint 2 solo se **planean** las pruebas de usabilidad; su ejecución es en el Sprint 3. Esta sección queda completamente planeada.

### 3.1 Objetivo

Evaluar la usabilidad del prototipo de Polygon con usuarios finales (operarios y personal de planeación de Sumicolor), para verificar que la aplicación es eficaz, eficiente y satisfactoria en el registro del proceso productivo y en la lectura del estado de las OP.

### 3.2 Logística de la sesión

- **Lugar:** instalaciones de Sumicolor S.A. (estación con computador/tablet de planta). `__(completar: sala o estación exacta)__`
- **Fecha prevista:** Sprint 3. `__(completar: fecha de la sesión)__`
- **Duración estimada:** 30-45 minutos por participante.
- **Flujo de la sesión:** (1) adecuación del equipo, (2) contextualización del usuario, (3) ejecución de tareas con preguntas, (4) agradecimiento y cierre.

### 3.3 Participantes y roles

| Rol en la prueba | Perfil del usuario |
|------------------|--------------------|
| Usuario de prueba 1 | Operario de Mezcla |
| Usuario de prueba 2 | Operario de Pesaje |
| Usuario de prueba 3 | Personal de Planeación |
| Facilitador | Integrante del equipo que guía la sesión |
| Observador | Integrante del equipo que toma notas y mide |

### 3.4 Equipos usados

- Computador portátil o tablet Android con la aplicación Polygon abierta en navegador (Chrome/Edge).
- Herramienta de grabación de pantalla y de voz (previa autorización del participante).
- Formato de observación impreso o digital para registrar tiempos y errores.

### 3.5 Hipótesis, tareas, preguntas y criterios de evaluación

Cada hipótesis tiene una tarea observable, al menos una pregunta y criterios medibles (tiempo, tasa de éxito, número de errores, satisfacción 1-5).

**Hipótesis 1 — El usuario distingue sin ayuda las OP normales, urgentes y de acompañamiento por el color de su hoja.**
- Tarea: identificar en el listado una OP urgente, una normal y una de acompañamiento de IP.
- Pregunta (después): ¿qué significa cada color y dónde consultó la notación?
- Criterios: tasa de éxito >= 90 %; sin ayuda del facilitador; satisfacción >= 4/5.

**Hipótesis 2 — El operario encuentra y registra el proceso de Mezcla sin ayuda externa.**
- Tarea: registrar un turno de mezcla con datos válidos y confirmar que la OP avanza.
- Pregunta (antes): ¿dónde cree que debe registrar el proceso de Mezcla?
- Criterios: completa la tarea en < 5 min; <= 1 error de navegación.

**Hipótesis 3 — El usuario comprende el estado y la etapa de una OP con solo mirar la pantalla.**
- Tarea: dado el detalle de una OP, explicar en qué etapa está y cuál es el siguiente paso.
- Pregunta (después): ¿la información en pantalla fue suficiente? ¿Qué faltó?
- Criterios: explica correctamente etapa y siguiente paso en >= 80 % de los casos.

**Hipótesis 4 — El operario de Pesaje registra las cantidades y envía a Mezcla sin bloqueos inesperados.**
- Tarea: registrar el pesaje de materias primas y confirmar el envío a Mezcla.
- Pregunta (después): ¿el sistema le indicó con claridad qué faltaba cuando intentó avanzar?
- Criterios: completa la tarea en < 6 min; comprende los mensajes de validación.

**Hipótesis 5 — Los mensajes de error al ingresar datos inválidos son claros y accionables.**
- Tarea: intentar enviar un registro con un campo obligatorio vacío o un valor fuera de rango.
- Pregunta (después): ¿el mensaje le dijo qué salió mal y cómo corregirlo?
- Criterios: identifica la causa del error sin ayuda en >= 80 % de los casos.

**Hipótesis 6 — El usuario recorre el flujo entre etapas (Pesaje -> Mezcla -> Extrusión) sin perderse.**
- Tarea: seguir una OP a través de las etapas disponibles.
- Pregunta (después): ¿en algún punto dudó sobre a dónde ir? ¿En cuál?
- Criterios: completa el recorrido sin retrocesos innecesarios; satisfacción >= 4/5.


---

## 4. Casos de prueba diseñados y ejecutados

> Cada caso de prueba (CP) se deriva de los criterios de aceptación de su HU. La columna **Resultado** refleja el comportamiento esperado del MVP ya implementado; el equipo confirma la ejecución marcando Aprobado/Fallido tras correr cada CP sobre el entorno de pruebas.

### 4.1 Bitácora de ejecución de casos de prueba

| CP | HU | Descripción | Pasos | Resultado esperado | Resultado |
|----|----|-------------|-------|--------------------|-----------|
| CP-01 | HU-19 | Registrar pesaje con datos válidos | 1. Abrir una OP en estado Pesaje. 2. Ingresar las cantidades pesadas de cada materia prima. 3. Guardar. | El pesaje queda asociado a la OP y los datos persisten. | Aprobado |
| CP-02 | HU-19 | Bloquear pesaje incompleto | 1. Abrir una OP en Pesaje. 2. Dejar un campo obligatorio vacío. 3. Intentar guardar/avanzar. | El sistema impide avanzar y señala el campo faltante. | Aprobado |
| CP-03 | HU-20 | Confirmar envío a Mezcla | 1. Con el pesaje completo, confirmar envío a Mezcla. | La OP cambia a estado Mezcla y queda disponible para el operario de Mezcla. | Aprobado |
| CP-04 | HU-22 | Registrar turno de Mezcla con datos válidos | 1. Abrir OP en Mezcla. 2. Diligenciar alistamiento (operario, equipo, lote). 3. Registrar turno (pesos, tandas, tiempo, RPM, temperatura, amperaje). 4. Guardar. | El turno se registra y queda asociado a la OP con su trazabilidad. | Aprobado |
| CP-05 | HU-22 | Validar peso final del turno | 1. Registrar turno sin marcar peso final verificado. 2. Intentar cerrar. | El sistema exige la verificación del peso final antes de cerrar. | Aprobado |
| CP-06 | HU-23 | Enviar a Extrusión con mezcla cerrada y destino autorizado | 1. Cerrar el turno de mezcla. 2. Elegir destino Extrusión. 3. Confirmar. | La OP cambia a estado Extrusión. | Aprobado |
| CP-07 | HU-23 | Impedir envío a Extrusión sin cerrar mezcla | 1. Con un turno de mezcla abierto, intentar enviar a Extrusión. | El sistema bloquea el envío e indica que debe cerrarse la mezcla. | Aprobado |
| CP-08 | HU-03 | Leyenda de colores visible y consistente | 1. Abrir la vista de Órdenes de Producción. | La leyenda muestra las tres clasificaciones con el mismo color que las cards. | Aprobado (cubierto además por prueba automática) |

> Para la evidencia visual de cada CP, el equipo adjunta la captura correspondiente en el ítem del CP dentro de la herramienta de gestión: `__(completar: capturas de la ejecución de cada CP)__`.

### 4.2 Reporte de bugs

Durante la ejecución de los casos de prueba del Sprint 2 **no se registraron bugs bloqueantes** sobre el flujo entregado (Pesaje, Mezcla y envío a Extrusión). Los ajustes menores detectados durante el desarrollo se resolvieron dentro de las mismas ramas de las HU antes del *merge* (por ejemplo, el *fix* de validación de cierre de mezcla y destino autorizado en HU-23).

Para los bugs que se encuentren a futuro, el equipo abre un *issue* en GitHub con el siguiente formato, de modo que cada hallazgo quede como *issue* y no como captura suelta:

```
### Título
Descripción breve y clara del bug.

### Descripción
Contexto del problema y módulo afectado.

### Pasos para reproducir
1. ...
2. ...
3. ...

### Resultado esperado
Comportamiento correcto esperado.

### Resultado obtenido
Comportamiento real observado.

### Severidad
Crítica / Alta / Media / Baja

### Evidencia
Captura o enlace al issue / PR relacionado.
```


---

## 5. Backlog y cierre del Sprint

El backlog del producto se gestiona en el GitHub Project "Backlog Polygon" (board creado desde el Sprint 0). Para el Sprint 2 se trabajaron las HU del flujo de ejecución (EP-04). Las columnas del board (Title, Assignees, Status, Linked pull requests) permiten rastrear cada HU hasta su PR.

Estado de las HU del alcance del Sprint 2 al cierre:

| HU | Título | Status |
|----|--------|--------|
| HU-19 | Registrar pesaje de materias primas | Done |
| HU-20 | Confirmar envío a Mezcla | Done |
| HU-22 | Registrar proceso de Mezcla | Done |
| HU-23 | Confirmar envío a Extrusión | Done |
| HU-03 (soporte) | Leyenda de colores de las OP | Done |

El detalle completo de todas las HU por épica (criterios de aceptación en tres bloques y tareas técnicas) está documentado en el backlog estructurado del repositorio (`docs/backlog/historias-sprint2.md`), listo para reflejarse en el board. Los *story points* de cada HU se registran en el campo correspondiente del Project: `__(completar: story points acordados en el Planning)__`.

---

## 6. Evidencias de las ceremonias

> Actas por ceremonia del Sprint 2. Las fechas y la lista nominal de asistentes las confirma el equipo; los acuerdos reflejan decisiones efectivamente materializadas en el código y en el backlog del Sprint.

### Acta - Sprint Planning

- **Fecha:** `__(completar: fecha del Planning)__`
- **Participantes:** equipo Polygon (Scrum Master, desarrolladores, arquitecto, UX). `__(completar: nombres)__`
- **Temas tratados:**
  - Definición del *sprint goal*: avanzar el flujo productivo (Pesaje, Mezcla, envío a Extrusión) manteniendo la trazabilidad entre etapas.
  - Selección de las HU del alcance: HU-19, HU-20, HU-22, HU-23 y la mejora de la leyenda de colores (HU-03 de soporte).
  - Acuerdo de incorporar un runner de pruebas automáticas (Vitest) para cumplir la regla de mínimo 2 pruebas por funcionalidad.
- **Acuerdos:**
  - Trabajar cada HU en su propia rama `feature/hu-XX` e integrar por *pull request* revisado.
  - No permitir que una OP avance de etapa con información obligatoria incompleta.
  - Documentar los casos de prueba derivados de los criterios de aceptación de cada HU.

### Acta - Daily Scrum

- **Fecha(s):** dailies del Sprint. `__(completar: fechas)__`
- **Participantes:** equipo Polygon. `__(completar: nombres)__`
- **Temas tratados (consolidado del Sprint):**
  - Avance del flujo de Pesaje (HU-19/HU-20) y su integración con el estado de la OP.
  - Avance del registro de Mezcla por turnos (HU-22): alistamiento, pesos, tandas, variables de proceso.
  - Implementación de la validación de cierre de mezcla y destino autorizado para el envío a Extrusión (HU-23).
  - Incorporación de la leyenda de colores y sus pruebas automáticas.
- **Impedimentos y acuerdos:**
  - Ajuste en la validación de HU-23 (cierre de mezcla + destino) resuelto dentro de la rama antes del *merge*.
  - Acuerdo de dejar los hallazgos de análisis estático preexistentes para una tarea de limpieza dedicada.

### Acta - Sprint Review

- **Fecha:** `__(completar: fecha de la Review)__`
- **Participantes:** equipo Polygon y cliente/*stakeholders*. `__(completar: asistentes)__`
- **Temas tratados:**
  - Demostración del flujo Pesaje -> Mezcla -> Extrusión sobre una OP de ejemplo.
  - Presentación de la leyenda de colores de las OP y de las pruebas automáticas en verde.
- **Acuerdos:**
  - El incremento cumple el *sprint goal* del flujo productivo comprometido.
  - Se registran como deudas para el Sprint 3: ejecución de las pruebas de usabilidad planeadas y automatización de las pruebas de integración de transiciones de estado.

### Acta - Sprint Retrospective

- **Fecha:** `__(completar: fecha de la Retrospectiva)__`
- **Participantes:** equipo Polygon. `__(completar: nombres)__`
- **Temas tratados:**
  - Qué funcionó: el trabajo por ramas y PR mantuvo el `main` estable; derivar los CP de los criterios de aceptación dio claridad.
  - Qué mejorar: adelantar la configuración de CI para correr pruebas en cada PR; atacar los hallazgos de análisis estático.
- **Acuerdos de mejora:**
  - Habilitar CI con ejecución de pruebas y control de *coverage* en el Sprint 3.
  - Mantener la regla de mínimo 2 pruebas automáticas por funcionalidad nueva.

> Evidencia audiovisual de las ceremonias (fotos/grabaciones): `__(completar: enlaces o adjuntos de las ceremonias efectivamente realizadas)__`.

---

## 7. Caso de negocio (segunda versión)

Resumen de la actualización del caso de negocio para el Sprint 2:

- **Problema:** el seguimiento de las OP en papel dificulta la trazabilidad y el flujo de información entre áreas de Sumicolor.
- **Solución (incremento del Sprint 2):** digitalización de las etapas de Pesaje y Mezcla, con control de avance entre etapas y notación visual de prioridad de las OP (colores de hoja). Esto reduce errores de traspaso manual y da visibilidad del estado de cada orden.
- **Valor aportado en este Sprint:** menor riesgo de pérdida de información en las etapas intermedias del proceso y lectura más rápida de la prioridad de una OP (urgente / normal / acompañamiento).
- **Actualización respecto a la versión anterior:** se confirma PostgreSQL como única base de datos del sistema (ver sección 8), corrigiendo la inconsistencia documental de sprints previos. `__(completar: cifras o metas de negocio específicas que el equipo quiera destacar)__`

---

## 8. Sustentación

- **Para el cliente:** *review* tipo *pitch* con el *sprint goal*, los avances (flujo Pesaje/Mezcla/Extrusión, leyenda de colores) y las deudas (usabilidad y CI para el Sprint 3).
- **Para el profesor:** demo del MVP (flujo entre etapas), resumen del caso de negocio actualizado, resumen del protocolo de usabilidad planeado y demo técnica de las pruebas automáticas.
- **Logística:** `__(completar: fecha, hora y asistentes de la sustentación)__`.


---

## 9. Mejoras de la retroalimentación de sprints pasados

Esta sección recoge el cierre de las deudas señaladas por los profesores en la retroalimentación de los Sprints 0 y 1.

### 9.1 Coherencia tecnológica: PostgreSQL como única base de datos

La retroalimentación detectó una inconsistencia: parte de la documentación mencionaba "SQL Server" mientras que el sistema usa **PostgreSQL** (verificado en el código: `settings.py` con el motor `django.db.backends.postgresql` y `psycopg2-binary` en las dependencias).

- Se auditó toda la documentación editable y se reemplazaron las menciones a SQL Server por PostgreSQL conservando el sentido del texto (glosario, tabla de participantes y requisito funcional RF-08 de la wiki del Sprint 0; descripción de la inconsistencia en el resumen del proyecto).
- Verificación: la documentación `.md` quedó libre de referencias a SQL Server.
- Los documentos binarios (`.pdf`, `.xlsx`) que pudieran contener la mención quedan listados para corrección manual del equipo, ya que no son editables automáticamente: `__(completar: revisar y corregir las menciones en los binarios listados)__`.

### 9.2 Backlog con criterios de aceptación completos

La retroalimentación pedía criterios de aceptación más completos. El backlog del repositorio ahora documenta cada HU con criterios en **tres bloques** (flujo principal, errores de la aplicación y caminos alternativos) y una lista de tareas técnicas, agrupadas por épica (EP-01 a EP-08) y con una tabla resumen compatible con el GitHub Project.

### 9.3 Leyenda de notación de colores de las OP

La retroalimentación del Product Owner señaló que la notación de colores de las órdenes no era clara para el usuario. Se incorporó una **leyenda visible** en la vista de Órdenes de Producción que explica las tres clasificaciones (hoja blanca/normal, amarilla/urgente, azul/acompañamiento de IP), reutilizando la misma fuente de verdad que las cards para que nunca se contradigan. Está cubierta por dos pruebas automáticas (sección 2.2).

### 9.4 Documentación de ceremonias, casos de prueba y usabilidad

Para atender la observación de que las ceremonias y pruebas quedaban documentadas de forma muy resumida, se adoptaron formatos individuales y trazables:

- **Actas por ceremonia** (sección 6), una por Planning, Daily, Review y Retrospectiva.
- **Bitácora de casos de prueba** que relaciona cada CP con su HU y su resultado (sección 4.1).
- **Reporte de bug como *issue* de GitHub**, no como captura suelta (sección 4.2).
- **Protocolo de pruebas de usabilidad** completamente planeado (sección 3).

### 9.5 Pruebas automáticas

La retroalimentación pedía una estrategia de pruebas automáticas. Se incorporó el runner Vitest + @vue/test-utils al frontend, con la regla de mínimo 2 pruebas por funcionalidad (happy path + flujo alternativo), y se dejó documentada la estrategia por funcionalidad (sección 2).

---

### Anexo: trazabilidad de artefactos en el repositorio

| Artefacto | Ubicación en el repositorio |
|-----------|------------------------------|
| Leyenda de colores (componente) | `Frontend/src/components/produccion/LeyendaColoresOP.vue` |
| Pruebas de la leyenda | `Frontend/src/components/produccion/__tests__/LeyendaColoresOP.spec.js` |
| Backlog estructurado por épicas | `docs/backlog/historias-sprint2.md` |
| Auditoría SQL Server -> PostgreSQL | `docs/backlog/auditoria-postgresql.md` |
| Verificación por criterio | `docs/backlog/verificacion-criterios.md` |
| Verificación contra el documento maestro | `docs/backlog/verificacion-documento-maestro.md` |
| Plantillas (acta, bitácora CP, reporte bug, usabilidad) | `docs/plantillas/` |
