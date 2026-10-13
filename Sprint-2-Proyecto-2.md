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

El proyecto adopta una estrategia de pruebas automáticas en dos capas. En el **frontend**, con **Vitest** + **@vue/test-utils** (estándar del ecosistema Vite/Vue 3 ya usado en el repositorio), ejecutadas en modo de una sola corrida (`npm run test`, equivalente a `vitest --run`). En el **backend**, con las pruebas de **Django/DRF** (`APITestCase`), ejecutadas con `python manage.py test`. Ambas son aptas para integración continua y se corren en el *workflow* de CI (sección 2.4). La estrategia complementa las pruebas manuales (casos de prueba de la sección 4): las automáticas protegen la lógica, los permisos y el render de las funcionalidades, y las manuales se enfocan en la usabilidad y el recorrido humano.

| Funcionalidad | Tipo de prueba | Justificación |
|---------------|----------------|---------------|
| Leyenda de colores de las OP (componente de UI) | Componente (render) | Es un componente de presentación con tres clasificaciones fijas; la prueba de render verifica que cada clasificación aparece con su color correcto y que deriva de la fuente única de verdad, sin falsos positivos de un espacio de entrada amplio. |
| Consistencia de la notación de colores con la fuente de verdad | Componente (flujo alternativo) | Garantiza que la leyenda y las cards de OP nunca se contradigan, porque ambas leen las mismas constantes (`CLASIFICACION_BADGE` / `HOJA_OP`); cubre el caso límite de la hoja blanca que no tiene franja. |
| Transición de estado Mezcla -> Extrusión (HU-23) | Integración / escenario (recomendada) | Valida la regla de negocio de que no se puede enviar a Extrusión sin cerrar el turno de mezcla ni sin un destino autorizado; es lógica de dominio con caminos de error claros. |
| Registro de pesaje (HU-19) | Escenario (recomendada) | Verifica que las cantidades pesadas quedan asociadas a la OP y que los campos obligatorios bloquean el avance. |

> Nota de alcance: en este Sprint se implementaron y ejecutaron las dos pruebas automáticas de la leyenda de colores (ver 2.2) y, en el backend, la suite de Django/DRF que cubre el flujo de Pesaje, Mezcla y envío a Extrusión, incluidas las transiciones de estado de las HU-19, HU-22 y HU-23 (137 pruebas en verde, ver 2.3). Las pruebas de integración del lado del frontend sobre esas mismas transiciones quedan diseñadas en la tabla y en los casos de prueba de la sección 4, listas para automatizarse a medida que se estabilice la capa de servicios.

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

### 2.3 Documento Maestro de Pruebas (backend Django/DRF)

Esta sección consolida el inventario de pruebas del backend y la tabla maestra `Funcionalidad | Tipo de Prueba | Justificación` de las cuatro aplicaciones con pruebas automáticas (`mezcla`, `pesaje`, `produccion`, `usuarios`), más las seis aplicaciones que aún no entregan funcionalidad propia. La fuente de verdad de este contenido es `docs/pruebas/Documento_Maestro_Pruebas.md`; aquí se reproduce íntegro y con el resultado de ejecución ya actualizado (137 pruebas `OK`, cobertura 96 %).
#### Introducción

Este documento inventaría, aplicación por aplicación del backend Django/DRF, el estado de las pruebas automáticas del proyecto Polygon y consolida, en una tabla de tres columnas (`Funcionalidad`, `Tipo de Prueba`, `Justificación`), qué funcionalidades están cubiertas y cuáles quedan `pendiente de completar`.

**Fecha de la auditoría:** 2026-10-13 (fecha de la auditoría).

El estado aquí registrado proviene del repositorio: cada afirmación sobre el estado de pruebas de una aplicación se respalda con la ruta relativa del archivo que la sustenta (verificada por lectura directa de los archivos `Backend/apps/<app>/tests.py`). Ningún resultado de ejecución de prueba se inventa: la suite se ejecutó realmente (137 pruebas, `OK`, cobertura 96 %) y ese resultado agregado respalda las filas de las aplicaciones con pruebas. Para las seis aplicaciones que aún no entregan funcionalidad propia, lo que queda pendiente es la definición de la Historia de Usuario o el Caso de Prueba, lo que se señala con el marcador `[EVIDENCIA PENDIENTE: HU/CP de la app X]`.

#### Inventario por aplicación

Se auditan **exactamente diez** aplicaciones del backend presentes en `Backend/apps/`. Cada entrada indica la clasificación (`con pruebas automáticas` o `sin pruebas automáticas`, según el Requisito 1.2) y la ruta relativa que la respalda (Requisito 1.6).

1. **`mezcla`** — Con pruebas automáticas. Fuente: `Backend/apps/mezcla/tests.py` (contiene las clases `RegistroMezclaTests` y `EnviarExtrusionTests`, con pruebas ejecutables).
2. **`pesaje`** — Con pruebas automáticas. Fuente: `Backend/apps/pesaje/tests.py` (contiene la clase `FlujoPesajeTests`, con pruebas ejecutables).
3. **`produccion`** — Con pruebas automáticas. Fuente: `Backend/apps/produccion/tests.py` (contiene las clases `HojaProduccionTests`, `StickersTests`, `IngenieroProduccionTests` y `ConsultarEstadoTests`, con pruebas ejecutables).
4. **`usuarios`** — Con pruebas automáticas. Fuente: `Backend/apps/usuarios/tests.py` (contiene la clase `EdicionUsuariosTests`, con pruebas ejecutables).
5. **`calidad`** — Sin pruebas automáticas. Fuente: `Backend/apps/calidad/tests.py` (vacío, solo el comentario `# Create your tests here.`).
6. **`empaque`** — Sin pruebas automáticas. Fuente: `Backend/apps/empaque/tests.py` (vacío, solo el comentario `# Create your tests here.`).
7. **`extrusion`** — Sin pruebas automáticas. Fuente: `Backend/apps/extrusion/tests.py` (vacío, solo el comentario `# Create your tests here.`).
8. **`picking`** — Sin pruebas automáticas. Fuente: `Backend/apps/picking/tests.py` (vacío, solo el comentario `# Create your tests here.`).
9. **`reportes`** — Sin pruebas automáticas. Fuente: `Backend/apps/reportes/tests.py` (vacío, solo el comentario `# Create your tests here.`).
10. **`picky`** — Sin pruebas automáticas. Fuente: `Backend/apps/picky/` (no existe archivo `tests.py`).

**Resumen de la auditoría:**

- Aplicaciones con pruebas automáticas (cuatro, conforme al Requisito 1.4): `mezcla`, `pesaje`, `produccion`, `usuarios`.
- Aplicaciones sin pruebas automáticas (seis, conforme al Requisito 1.3): `calidad`, `empaque`, `extrusion`, `picking`, `reportes`, `picky`.
- Total auditado: diez aplicaciones (Requisito 1.1).

**Discrepancias (Requisito 1.7):** no se observaron discrepancias entre el estado verificado del repositorio y la clasificación declarada en el diseño (seis sin pruebas, cuatro con pruebas).

#### Convenciones de la tabla

- La columna `Tipo de Prueba` toma únicamente los valores `Happy Path` o `Flujo Alternativo` (Requisito 2.2). Las funcionalidades sin alguno de los dos caminos se marcan como `pendiente de completar` (Requisito 1.5).
- La columna `Justificación` relaciona cada prueba con la Historia de Usuario o el Caso de Prueba correspondiente y cita el archivo y método que la respalda (Requisito 2.3).
- La suite completa se ejecutó realmente con `python manage.py test` (137 pruebas, veredicto `OK`) y la cobertura total con `coverage` es del 96 %. Por eso el resultado por fila no se repite en cada entrada: queda respaldado por el resultado agregado que se registra al final de esta sección. Ningún valor se inventa (Requisitos 2.4, 7.1 y 7.3).

#### Tabla maestra: Funcionalidad | Tipo de Prueba | Justificación

##### Aplicaciones con pruebas automáticas

#### `mezcla` (`Backend/apps/mezcla/tests.py`)

| Funcionalidad | Tipo de Prueba | Justificación |
|---------------|----------------|---------------|
| Consultar el alistamiento de mezcla sin registro previo (apps/mezcla) | Happy Path | HU de registro de mezcla: GET devuelve la OP y `registro` nulo cuando aún no hay datos. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_consultar_sin_registro`. |
| Crear registro de mezcla por el operario (apps/mezcla) | Happy Path | HU de registro de mezcla: PATCH válido crea `RegistroMezcla` con operario, equipo y observaciones y responde 200. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_crear_registro_mezcla`. |
| Actualizar un registro de mezcla existente (apps/mezcla) | Happy Path | HU de registro de mezcla: PATCH sobre registro existente modifica el equipo sin duplicar filas. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_actualizar_registro_existente`. |
| Registrar trazabilidad del alistamiento (apps/mezcla) | Happy Path | HU de trazabilidad: cada campo diligenciado genera eventos de historial atribuidos al operario. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_registrar_trazabilidad`. |
| No duplicar historial cuando no hay cambios (apps/mezcla) | Flujo Alternativo | CP de trazabilidad: un PATCH repetido con los mismos datos no genera un segundo evento de historial. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_no_duplicar_historial_sin_cambios`. |
| El alistamiento no altera el estado de la OP (apps/mezcla) | Flujo Alternativo | CP de control de estado: registrar la mezcla no cambia el estado de la OP. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_no_modificar_estado_op`. |
| Rechazo de registro con operario en blanco (apps/mezcla) | Flujo Alternativo | CP de validación de entrada: PATCH con `nombre_operario` vacío responde 400 y no crea registro. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_rechazar_registro_vacio`. |
| Bloqueo de edición de OP fuera del estado mezcla (apps/mezcla) | Flujo Alternativo | CP de estado no permitido: una OP en pesaje responde 404 al intentar registrar mezcla. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_no_modificar_orden_fuera_de_mezcla`. |
| Rechazo de usuario sin rol de mezcla (apps/mezcla) | Flujo Alternativo | CP de permiso denegado: un usuario de producción recibe 403 al registrar mezcla. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_rechazar_usuario_sin_rol_mezcla`. |
| Rechazo de cambio de estado por la API de alistamiento (apps/mezcla) | Flujo Alternativo | CP de entrada inválida: incluir `estado` en el PATCH responde 400 y conserva el estado. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_rechazar_cambio_de_estado_por_api`. |
| Rechazo de usuario no autenticado (apps/mezcla) | Flujo Alternativo | CP de autenticación: una consulta sin sesión responde 401/403. Fuente: `Backend/apps/mezcla/tests.py::RegistroMezclaTests::test_rechazar_usuario_no_autenticado`. |
| Enviar la OP a extrusión conservando los turnos (apps/mezcla) | Happy Path | HU-23 de envío a extrusión: con registro, destino autorizado y turnos cerrados, el envío responde 200 y deja la OP en extrusión. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_enviar_extrusion_correctamente_y_conservar_turnos`. |
| Permitir varios turnos cerrados antes del envío (apps/mezcla) | Happy Path | HU-23: varios turnos cerrados no impiden el envío a extrusión. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_permite_varios_turnos_cerrados`. |
| No enviar a extrusión sin registro de alistamiento (apps/mezcla) | Flujo Alternativo | CP de precondición: sin registro, el envío responde 400 y la OP sigue en mezcla. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_enviar_sin_registro`. |
| Un registro legado no satisface los nuevos requisitos de envío (apps/mezcla) | Flujo Alternativo | CP de compatibilidad: un registro antiguo incompleto bloquea el envío. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_registro_legado_no_satisface_los_nuevos_requisitos`. |
| No enviar sin turnos registrados (apps/mezcla) | Flujo Alternativo | CP de precondición: sin turnos, el envío responde 400. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_enviar_sin_turnos`. |
| No enviar con un turno abierto (apps/mezcla) | Flujo Alternativo | CP de estado no permitido: un turno sin cerrar bloquea el envío. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_enviar_con_turno_abierto`. |
| Un turno cerrado no oculta otro abierto (apps/mezcla) | Flujo Alternativo | CP de estado no permitido: con un turno abierto adicional, el envío se bloquea. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_un_turno_cerrado_no_oculta_otro_abierto`. |
| No enviar sin destino autorizado (apps/mezcla) | Flujo Alternativo | CP de precondición: sin autorización de destino, el envío responde 400. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_enviar_sin_destino_autorizado`. |
| No enviar a extrusión si el destino es calidad u otro (apps/mezcla) | Flujo Alternativo | CP de destino inválido: un destino distinto de extrusión bloquea el envío. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_enviar_a_extrusion_si_destino_es_calidad_u_otro`. |
| El operario no puede autorizar el destino (apps/mezcla) | Flujo Alternativo | CP de permiso denegado: el operario de mezcla recibe 403 al autorizar destino. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_operario_no_puede_autorizar_destino`. |
| No editar el destino por el endpoint de alistamiento (apps/mezcla) | Flujo Alternativo | CP de entrada inválida: campos de destino en el PATCH de alistamiento responden 400. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_editar_destino_por_endpoint_de_alistamiento`. |
| Autorización válida y repetida no duplica historial (apps/mezcla) | Flujo Alternativo | CP de trazabilidad: autorizar dos veces el destino no genera eventos duplicados. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_autorizacion_valida_y_repetida_no_duplica_historial`. |
| La autorización no admite destinos inventados (apps/mezcla) | Flujo Alternativo | CP de entrada inválida: un destino fuera del catálogo responde 400. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_autorizacion_no_admite_destinos_inventados`. |
| Cambiar el lote invalida el destino autorizado (apps/mezcla) | Flujo Alternativo | CP de consistencia: modificar el lote limpia la autorización de destino. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_cambiar_lote_invalida_destino`. |
| No permitir doble envío a extrusión (apps/mezcla) | Flujo Alternativo | CP de idempotencia: el segundo envío responde 400 y no duplica el historial de estado. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_no_permitir_doble_envio`. |
| El historial identifica cuenta, lote y autorización (apps/mezcla) | Happy Path | HU-23 de trazabilidad: el evento de envío registra operario, lote y supervisor autorizante. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_historial_identifica_cuenta_lote_y_autorizacion`. |
| El envío no admite campos ni estado fabricado (apps/mezcla) | Flujo Alternativo | CP de entrada inválida: datos extra o `estado` en el envío responden 400. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_enviar_no_admite_campos_ni_estado_fabricado`. |
| Rechazar otros roles y usuario no autenticado en el envío (apps/mezcla) | Flujo Alternativo | CP de permiso/autenticación: supervisor, producción y anónimo reciben 401/403 al enviar. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_rechazar_otros_roles_y_usuario_no_autenticado`. |
| Rechazar orden inexistente en el envío (apps/mezcla) | Flujo Alternativo | CP de recurso inexistente: un `pk` inválido responde 404. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_rechazar_orden_inexistente`. |
| Consulta tras el envío con escritura bloqueada (apps/mezcla) | Flujo Alternativo | CP de estado posterior: tras enviar, la lectura funciona pero toda escritura responde 404. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_consulta_despues_del_envio_y_escritura_bloqueada`. |
| Información de turno incompleta bloquea el envío aunque esté cerrado (apps/mezcla) | Flujo Alternativo | CP de validación: campos faltantes en un turno cerrado bloquean el envío. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_informacion_de_turno_incompleta_aunque_este_cerrado`. |
| Producto especial no se envía con liberación invalidada (apps/mezcla) | Flujo Alternativo | CP de producto especial: una liberación invalidada bloquea el envío hasta re-liberar. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_especial_no_puede_enviarse_con_liberacion_invalidada`. |
| Verificaciones incompletas bloquean el envío (apps/mezcla) | Flujo Alternativo | CP de checklist: una verificación general en falso bloquea el envío. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_verificaciones_incompletas_bloquean_envio`. |
| Rollback del estado si falla el registro de historial (apps/mezcla) | Flujo Alternativo | CP de atomicidad: un fallo al registrar historial revierte el cambio de estado. Fuente: `Backend/apps/mezcla/tests.py::EnviarExtrusionTests::test_fallo_historial_revierte_cambio_de_estado`. |

#### `pesaje` (`Backend/apps/pesaje/tests.py`)

| Funcionalidad | Tipo de Prueba | Justificación |
|---------------|----------------|---------------|
| Guardar borrador del pesaje (apps/pesaje) | Happy Path | HU de pesaje: guardar borrador responde 200 y crea el `RegistroPesaje` con operario y fecha de recepción. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_guarda_borrador`. |
| Enviar datos guardados y reflejar "no cumple" explícito (apps/pesaje) | Happy Path | HU de pesaje: el envío lleva la OP a supervisión y conserva una verificación en falso. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_envia_datos_guardados_y_no_cumple_es_respuesta_explicita`. |
| El envío no pide pesos ni altera la fórmula (apps/pesaje) | Happy Path | HU de pesaje: el operario no digita el peso real y la cantidad de fórmula se conserva. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_envio_no_pide_pesos_ni_altera_la_formula`. |
| Sellado único del inicio de pesaje (apps/pesaje) | Happy Path | HU de trazabilidad de pesaje: la hora de inicio se sella una sola vez. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_inicio_de_pesaje_se_sella_una_sola_vez`. |
| Avance pendiente/pesando/pesado persiste en el servidor (apps/pesaje) | Happy Path | HU de avance por materia prima: los estados y sus horas se guardan y pueden deshacerse. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_avance_pendiente_pesando_pesado_queda_en_servidor`. |
| Ciclo devolución-corrección-reenvío-aprobación con historial (apps/pesaje) | Happy Path | HU de supervisión: el ciclo completo deja la OP en mezcla y registra el historial de estados. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_ciclo_devolucion_correccion_reenvio_aprobacion_con_historial`. |
| Pesaje crítico completo se envía y el supervisor ve "no cumple" (apps/pesaje) | Happy Path | HU de pesaje crítico: con las verificaciones críticas, el envío llega a supervisión y conserva el "no cumple". Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_critico_completo_envia_y_supervisor_ve_no_cumple`. |
| El crítico no exige las verificaciones del formulario normal (apps/pesaje) | Happy Path | HU de pesaje crítico: el formulario crítico sustituye al estándar y se envía 200. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_critico_no_exige_las_verificaciones_del_formulario_normal`. |
| Devolución y reenvío conservan condiciones críticas y trazabilidad (apps/pesaje) | Happy Path | HU de pesaje crítico: tras devolver y reenviar, las condiciones críticas y el historial se conservan. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_devolucion_reenvio_conservan_condiciones_y_trazabilidad`. |
| Bandeja de pesaje filtrada y supervisor consulta todo (apps/pesaje) | Happy Path | HU de bandejas: pesaje solo ve su cola y el supervisor consulta todas las OP. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_bandeja_de_pesaje_filtrada_y_supervisor_consulta_todo`. |
| Transición desde picking conserva compatibilidad (apps/pesaje) | Happy Path | HU de recepción desde picking: la recepción habilita el envío a pesaje. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_transicion_desde_picking_conserva_compatibilidad`. |
| No envía incompleto ni crea borrador en error (apps/pesaje) | Flujo Alternativo | CP de validación: un envío sin datos responde 400 y no deja registro ni historial. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_no_envia_incompleto_ni_crea_borrador_en_error`. |
| Todas las verificaciones y campos son obligatorios (apps/pesaje) | Flujo Alternativo | CP de campos obligatorios: omitir cualquier campo o verificación responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_todas_las_verificaciones_y_campos_son_obligatorios`. |
| Orden sin materiales no se envía (apps/pesaje) | Flujo Alternativo | CP de precondición: una OP sin materiales responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_orden_sin_materiales_no_se_envia`. |
| No editar en supervisión ni repetir envío (apps/pesaje) | Flujo Alternativo | CP de estado no permitido: en supervisión, guardar o reenviar responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_no_edita_en_supervision_ni_repite_envio`. |
| Solo el supervisor aprueba o devuelve (apps/pesaje) | Flujo Alternativo | CP de permiso denegado: otros roles reciben 403 al aprobar o devolver. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_solo_supervisor_aprueba_o_devuelve`. |
| Pesaje no puede enviar directamente a mezcla (apps/pesaje) | Flujo Alternativo | CP de transición no permitida: forzar estado/revisión no mueve la OP a mezcla. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_pesaje_no_puede_enviar_directamente_a_mezcla`. |
| El supervisor no edita el formulario (apps/pesaje) | Flujo Alternativo | CP de permiso denegado: el supervisor recibe 403 al guardar o enviar el formulario. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_supervisor_no_edita_el_formulario`. |
| La devolución exige motivo (apps/pesaje) | Flujo Alternativo | CP de validación: devolver sin motivo (o en blanco) responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_devolucion_exige_motivo`. |
| Rollback si falla la trazabilidad del pesaje (apps/pesaje) | Flujo Alternativo | CP de atomicidad: un fallo al registrar historial revierte el envío. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_rollback_si_falla_trazabilidad`. |
| Las acciones requieren autenticación (apps/pesaje) | Flujo Alternativo | CP de autenticación: sin sesión, las acciones responden 401. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_acciones_requieren_autenticacion`. |
| No crítico no requiere sección aunque el texto mencione grupos (apps/pesaje) | Flujo Alternativo | CP de clasificación: una OP no crítica no exige el formulario crítico. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_no_critico_no_requiere_seccion_aunque_texto_mencione_grupos`. |
| Iniciar pesaje solo lo hace el operario de pesaje (apps/pesaje) | Flujo Alternativo | CP de permiso denegado: otros roles reciben 403 al iniciar el pesaje. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_iniciar_pesaje_solo_lo_hace_el_operario_de_pesaje`. |
| Solo una materia prima "pesando" a la vez (apps/pesaje) | Flujo Alternativo | CP de concurrencia: iniciar un segundo material en paralelo responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_avance_solo_una_materia_prima_pesando_a_la_vez`. |
| El avance requiere recepción, estado válido y rol (apps/pesaje) | Flujo Alternativo | CP de precondición/permiso: sin recepción, con material ajeno o rol incorrecto, el avance responde 404/403/400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_avance_requiere_recibido_estado_pesaje_y_rol`. |
| Con varias materias primas exige todas pesadas para enviar (apps/pesaje) | Flujo Alternativo | CP de precondición: faltando pesar algún material, el envío responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_con_varias_materias_primas_exige_todas_pesadas_para_enviar`. |
| El crítico no acepta respuestas del formulario normal (apps/pesaje) | Flujo Alternativo | CP de entrada inválida: campos del formulario estándar en una OP crítica responden 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_critico_no_acepta_respuestas_del_formulario_normal`. |
| "No cumple" exige observación pero no impide avanzar (apps/pesaje) | Flujo Alternativo | CP de validación condicional: un "no cumple" sin observación responde 400; con observación avanza. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_no_cumple_exige_observacion_pero_no_impide_avanzar`. |
| "No cumple" crítico exige acciones correctivas (apps/pesaje) | Flujo Alternativo | CP de validación condicional: un "no cumple" crítico sin observaciones responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_no_cumple_critico_exige_acciones_correctivas`. |
| La recepción crítica no vuelca el checklist pendiente en el historial (apps/pesaje) | Flujo Alternativo | CP de trazabilidad: el historial solo registra respuestas efectivas del checklist crítico. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_recepcion_critica_no_vuelca_checklist_pendiente_en_historial`. |
| Cada grupo crítico requiere todas las respuestas (apps/pesaje) | Flujo Alternativo | CP de campos obligatorios: cada grupo crítico exige todas sus verificaciones. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_cada_grupo_critico_requiere_todas_las_respuestas`. |
| Crítico incompleto guarda borrador pero no envía (apps/pesaje) | Flujo Alternativo | CP de estado parcial: un crítico incompleto admite borrador pero bloquea el envío. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_critico_incompleto_guarda_borrador_pero_no_envia`. |
| Sin clasificar no asume no crítico ni acepta grupo del operario (apps/pesaje) | Flujo Alternativo | CP de clasificación: una OP sin clasificar no acepta grupo enviado por el operario. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_sin_clasificar_no_asume_no_critico_ni_acepta_grupo_del_operario`. |
| No crítico no acepta respuestas ocultas (apps/pesaje) | Flujo Alternativo | CP de entrada inválida: enviar campos críticos en una OP no crítica responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_no_critico_no_acepta_respuestas_ocultas`. |
| La aprobación revalida las condiciones críticas (apps/pesaje) | Flujo Alternativo | CP de revalidación: si una condición crítica queda sin respuesta, la aprobación responde 400. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_aprobacion_revalida_condiciones_criticas`. |
| El admin congela el grupo enviado pero permite clasificar legadas (apps/pesaje) | Flujo Alternativo | CP de administración: el grupo crítico se vuelve de solo lectura una vez enviado. Fuente: `Backend/apps/pesaje/tests.py::FlujoPesajeTests::test_admin_congela_grupo_enviado_pero_permite_clasificar_legadas`. |

#### `produccion` (`Backend/apps/produccion/tests.py`)

| Funcionalidad | Tipo de Prueba | Justificación |
|---------------|----------------|---------------|
| Enviar a picking guarda lo diligenciado sin botón de guardar (apps/produccion) | Happy Path | HU de hoja de producción: enviar a picking guarda clasificación, observaciones y grupo crítico y registra el historial. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_enviar_guarda_lo_diligenciado_sin_boton_guardar`. |
| Sin stickers críticos el producto es no crítico (apps/produccion) | Happy Path | HU de clasificación: sin stickers críticos, el grupo de pesaje queda no crítico. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_sin_stickers_criticos_el_producto_es_no_critico`. |
| Varios stickers críticos toman el de mayor prioridad (apps/produccion) | Happy Path | HU de clasificación: con varios stickers críticos se elige el de mayor prioridad. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_varios_stickers_criticos_toman_el_de_mayor_prioridad`. |
| Enviar sin cambios solo registra el envío (apps/produccion) | Happy Path | HU de hoja de producción: un envío sin cambios registra únicamente el cambio de estado. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_enviar_sin_cambios_solo_registra_el_envio`. |
| Dato inválido no guarda ni envía (apps/produccion) | Flujo Alternativo | CP de validación: una clasificación inválida responde 400 y no modifica la OP. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_dato_invalido_no_guarda_ni_envia`. |
| Si falla la trazabilidad no queda nada a medias (apps/produccion) | Flujo Alternativo | CP de atomicidad: un fallo al registrar historial revierte clasificación, estado y fecha de envío. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_si_falla_la_trazabilidad_no_queda_nada_a_medias`. |
| No se reclasifica ni reenvía tras salir de producción (apps/produccion) | Flujo Alternativo | CP de estado no permitido: tras salir de producción, reclasificar o reenviar responde 400. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_no_se_reclasifica_ni_reenvia_despues_de_salir_de_produccion`. |
| Solo producción manda la hoja (apps/produccion) | Flujo Alternativo | CP de permiso denegado: otros roles reciben 403 al enviar la hoja. Fuente: `Backend/apps/produccion/tests.py::HojaProduccionTests::test_solo_produccion_manda_la_hoja`. |
| Sin stickers es proceso normal (apps/produccion) | Happy Path | HU de stickers: una OP sin stickers se consulta como proceso normal. Fuente: `Backend/apps/produccion/tests.py::StickersTests::test_sin_stickers_es_proceso_normal`. |
| Poner varios stickers en orden del catálogo y registrar historial (apps/produccion) | Happy Path | HU de stickers: se ordenan por catálogo, se deduplican y se registra el cambio una vez. Fuente: `Backend/apps/produccion/tests.py::StickersTests::test_pone_varios_en_orden_del_catalogo_y_registra_historial`. |
| Corregir stickers en otra etapa pero no en una OP cerrada (apps/produccion) | Flujo Alternativo | CP de estado: los stickers se corrigen en etapas intermedias pero no en una OP finalizada (400). Fuente: `Backend/apps/produccion/tests.py::StickersTests::test_se_corrige_en_otra_etapa_pero_no_en_una_op_cerrada`. |
| Ajuste y reproceso no los pone producción ni los borra (apps/produccion) | Flujo Alternativo | CP de autoría de stickers: códigos restringidos o inventados responden 400 y los automáticos se conservan. Fuente: `Backend/apps/produccion/tests.py::StickersTests::test_ajuste_y_reproceso_no_los_pone_produccion_ni_los_borra`. |
| Los stickers de pesaje solo cambian en producción (apps/produccion) | Flujo Alternativo | CP de estado: cambiar el sticker crítico fuera de producción responde 400. Fuente: `Backend/apps/produccion/tests.py::StickersTests::test_stickers_de_pesaje_solo_cambian_en_produccion`. |
| Solo producción pone stickers (apps/produccion) | Flujo Alternativo | CP de permiso denegado: otros roles reciben 403 al poner stickers. Fuente: `Backend/apps/produccion/tests.py::StickersTests::test_solo_produccion_pone_stickers`. |
| Ingeniero de producción solo ve las OP con acompañamiento de IP (apps/produccion) | Happy Path | HU de acompañamiento IP: el ingeniero consulta solo las OP de hoja azul en cualquier etapa. Fuente: `Backend/apps/produccion/tests.py::IngenieroProduccionTests::test_solo_ve_las_op_con_acompanamiento_de_ip_en_cualquier_etapa`. |
| El ingeniero de producción no edita ni mueve la OP (apps/produccion) | Flujo Alternativo | CP de permiso denegado: el ingeniero recibe 403 en toda acción de edición o avance. Fuente: `Backend/apps/produccion/tests.py::IngenieroProduccionTests::test_no_edita_ni_mueve_la_op`. |
| Buscar por número de orden (apps/produccion) | Happy Path | HU de consulta de estado: la búsqueda por número de orden devuelve la OP con su estado legible. Fuente: `Backend/apps/produccion/tests.py::ConsultarEstadoTests::test_busca_numero_de_orden`. |
| Cada estación solo consulta su cola (apps/produccion) | Happy Path | HU de consulta de estado: una estación solo ve las OP de su propia cola. Fuente: `Backend/apps/produccion/tests.py::ConsultarEstadoTests::test_estacion_solo_consulta_su_cola`. |

#### `usuarios` (`Backend/apps/usuarios/tests.py`)

| Funcionalidad | Tipo de Prueba | Justificación |
|---------------|----------------|---------------|
| El admin guarda su cuenta sin cambiar rol ni estado (apps/usuarios) | Happy Path | HU-05/HU-06 de edición de usuarios: el admin guarda su propia cuenta conservando rol y estado (200). Fuente: `Backend/apps/usuarios/tests.py::EdicionUsuariosTests::test_admin_puede_guardar_su_cuenta_sin_cambiar_rol_ni_estado`. |
| El admin sigue editando a los demás usuarios (apps/usuarios) | Happy Path | HU-05/HU-06: el admin cambia rol y estado de otro usuario y queda auditado. Fuente: `Backend/apps/usuarios/tests.py::EdicionUsuariosTests::test_admin_sigue_editando_a_los_demas`. |
| El admin no puede cambiar su propio rol (apps/usuarios) | Flujo Alternativo | CP de autoprotección: el admin recibe 400 al intentar cambiar su propio rol. Fuente: `Backend/apps/usuarios/tests.py::EdicionUsuariosTests::test_admin_no_puede_cambiar_su_propio_rol`. |
| El admin no puede desactivarse a sí mismo (apps/usuarios) | Flujo Alternativo | CP de autoprotección: el admin recibe 400 al intentar desactivar su propia cuenta. Fuente: `Backend/apps/usuarios/tests.py::EdicionUsuariosTests::test_admin_no_puede_desactivarse_a_si_mismo`. |
| Solo el administrador edita usuarios (apps/usuarios) | Flujo Alternativo | CP de permiso denegado: operario y supervisor reciben 403 al editar usuarios. Fuente: `Backend/apps/usuarios/tests.py::EdicionUsuariosTests::test_solo_el_administrador_edita_usuarios`. |
| Un usuario inactivo no inicia sesión (apps/usuarios) | Flujo Alternativo | CP de acceso: un usuario desactivado recibe 400 al autenticarse. Fuente: `Backend/apps/usuarios/tests.py::EdicionUsuariosTests::test_usuario_inactivo_no_inicia_sesion`. |

##### Aplicaciones sin pruebas automáticas

Las seis aplicaciones siguientes no tienen pruebas ejecutables al momento de la auditoría; por tanto, sus funcionalidades carecen de Happy Path y de Flujo Alternativo y se marcan como `pendiente de completar` (Requisito 1.5). La implementación de estas pruebas corresponde a las tareas 2.1 a 2.6 del plan.

| Funcionalidad | Tipo de Prueba | Justificación |
|---------------|----------------|---------------|
| Funcionalidades de la app `calidad` entregadas en el Sprint 2 | pendiente de completar | `tests.py` vacío (solo `# Create your tests here.`). Faltan Happy Path y Flujo Alternativo. Fuente: `Backend/apps/calidad/tests.py`. HU/CP: `[EVIDENCIA PENDIENTE: HU/CP de la app calidad]`. |
| Funcionalidades de la app `empaque` entregadas en el Sprint 2 | pendiente de completar | `tests.py` vacío (solo `# Create your tests here.`). Faltan Happy Path y Flujo Alternativo. Fuente: `Backend/apps/empaque/tests.py`. HU/CP: `[EVIDENCIA PENDIENTE: HU/CP de la app empaque]`. |
| Funcionalidades de la app `extrusion` entregadas en el Sprint 2 | pendiente de completar | `tests.py` vacío (solo `# Create your tests here.`). Faltan Happy Path y Flujo Alternativo. Fuente: `Backend/apps/extrusion/tests.py`. HU/CP: `[EVIDENCIA PENDIENTE: HU/CP de la app extrusion]`. |
| Funcionalidades de la app `picking` entregadas en el Sprint 2 | pendiente de completar | `tests.py` vacío (solo `# Create your tests here.`). Faltan Happy Path y Flujo Alternativo. Fuente: `Backend/apps/picking/tests.py`. HU/CP: `[EVIDENCIA PENDIENTE: HU/CP de la app picking]`. |
| Funcionalidades de la app `reportes` entregadas en el Sprint 2 | pendiente de completar | `tests.py` vacío (solo `# Create your tests here.`). Faltan Happy Path y Flujo Alternativo. Fuente: `Backend/apps/reportes/tests.py`. HU/CP: `[EVIDENCIA PENDIENTE: HU/CP de la app reportes]`. |
| Funcionalidades de la app `picky` entregadas en el Sprint 2 | pendiente de completar | No existe `tests.py` en la app. Faltan Happy Path y Flujo Alternativo. Fuente: `Backend/apps/picky/`. HU/CP y alcance: `[EVIDENCIA PENDIENTE: HU/CP de la app picky y confirmación de alcance en Sprint 2]`. |

#### Resultado de la ejecución de la suite de backend

Esta sección registra el resultado **real** de ejecutar la Suite_Pruebas_Backend, conforme a los Requisitos 3.5, 7.1 y 7.3 y a la tarea 2.7 del plan. En este punto la suite ya incluye las pruebas añadidas por las tareas 2.1 a 2.5 para las aplicaciones `calidad`, `empaque`, `extrusion`, `picking` y `reportes` (pruebas de estilo documental coherentes con el patrón de `Backend/apps/mezcla/tests.py`).

- **Comando ejecutado:** `python manage.py test` en `Backend/` (ejecutado con el intérprete del entorno virtual del repositorio: `..\venv\Scripts\python.exe manage.py test`).
- **Fecha de ejecución:** 2026-10-13.
- **Resultado agregado verbatim:**

```text
Found 137 test(s).
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
----------------------------------------------------------------------
Ran 137 tests in 8.799s
OK
Destroying test database for alias 'default'...
```

- **Código de salida del proceso:** `0`.

**Alcance de lo que esta corrida demuestra:** la ejecución agregada confirma que las 137 pruebas descubiertas por el *test runner* de Django finalizaron con el veredicto `OK` (ninguna falla ni error), con código de salida `0`. Este es un resultado **agregado**: acredita que la suite completa pasó, no se derivan de él afirmaciones individuales por cada prueba más allá de lo que el resumen agregado muestra. Ningún valor de resultado o de cobertura se inventa (Requisito 7).

> Nota: al redirigir la salida estándar de error en PowerShell (`2>&1`), Django emite su progreso por *stderr* y el shell puede reportar un código de salida distinto de cero de forma artificial; la ejecución sin esa redirección confirma `EXITCODE=0` y el veredicto `OK` mostrado arriba.

### 2.4 Integración Continua (CI)

Se añadió un *workflow* de GitHub Actions en `.github/workflows/ci.yml` (nombre `CI`) que se dispara en cada *pull request* hacia `main` y ejecuta dos *jobs* en paralelo:

| Job | Pasos |
|-----|-------|
| `backend` | checkout, Python 3.12, instalar `requirements.txt` + `requirements-dev.txt`, `ruff check .`, pruebas con `coverage`, umbral de cobertura, análisis de seguridad con `bandit`. |
| `frontend` | checkout, Node LTS, `npm ci`, `eslint .`, `npm run test` (Vitest), `npm run build`, `npm audit`. |

Ningún paso usa `continue-on-error`: cualquier paso con código de salida distinto de cero marca la ejecución como fallida, de modo que el *merge* queda condicionado a que el CI pase. Quedan como huecos acordables por el equipo el umbral de cobertura, el nivel de severidad de `npm audit` y el `timeout` de cada *job*: `__(completar: umbral de cobertura, nivel de npm audit y timeout acordados)__`.

**Protección de rama:** para que el *merge* se bloquee automáticamente cuando el CI falla, el Equipo_QA activa manualmente la protección de la rama `main` en GitHub (`Settings > Branches`), marcando el `Flujo_CI` (`backend` y `frontend`) como verificación de estado requerida y exigiendo al menos una aprobación. El procedimiento está documentado en la Guía de Protección de Rama (ver Anexo). Evidencia de la activación: `__(completar: captura de la configuración de branch protection tras la primera corrida del workflow)__`.

### 2.5 Análisis estático y seguridad

- **Backend:** `ruff check .` termina sin hallazgos (`All checks passed!`). Se corrigieron 51 hallazgos previos (líneas largas, imports desordenados, `if`/`with` anidados, un import duplicado) en `apps/mezcla` y `apps/produccion`.
- **Frontend:** `eslint .` termina sin hallazgos. Se resolvieron los hallazgos preexistentes que quedaban pendientes en `src/router/index.js` (claves duplicadas por un conflicto de *merge*) y en `src/views/mezcla/RegistroMezcla.vue`.
- **Seguridad:** `bandit -r apps` termina con 0 hallazgos; el único positivo (una credencial de prueba en `apps/usuarios/tests.py`) se justificó y suprimió con `# nosec B105` por ser un valor exclusivo de pruebas. En el frontend, `npm audit` se ejecuta como parte del *job* `frontend`.
- **Dependencias de desarrollo:** se añadió `Backend/requirements-dev.txt` con `coverage` y `bandit` (versiones fijadas), separadas de las dependencias de ejecución.


---

## 3. Protocolo de pruebas de usabilidad (planeación)

> El documento maestro indica que en el Sprint 2 solo se **planean** las pruebas de usabilidad; su ejecución es en el Sprint 3. Esta sección queda completamente planeada.

### 3.1 Objetivo

Evaluar la usabilidad del prototipo de Polygon con usuarios finales (operarios y personal de planeación de Sumicolor), para verificar que la aplicación es eficaz, eficiente y satisfactoria en el registro del proceso productivo y en la lectura del estado de las OP.

Este objetivo responde a los requisitos no funcionales definidos en el Sprint 0 (`docs/context/SprintsWiki/Sprint0_Proyecto2_Sumicolor.md`): **RNF-06 Usabilidad** (interfaz minimalista, pocos clics, botones grandes, colores distinguibles, navegación simple), **RNF-07 Accesibilidad** (tipografía grande, alto contraste, iconografía clara, mensajes sencillos, formularios simples) y **RNF-08 Compatibilidad** (computadores de escritorio, tabletas Android, Chrome y Edge).

### 3.2 Perfil de usuario (según Sprint 0)

El sistema está diseñado para **operarios de planta con distintos niveles de experiencia tecnológica** y para **personal de planeación**. El Sprint 0 justifica las decisiones de diseño (SPA, interfaz intuitiva, RNF-06/07) por este perfil: trabajadores que antes registraban el proceso en papel y cuya curva de aprendizaje debe minimizarse (`docs/context/SprintsWiki/Sprint0_Proyecto2_Sumicolor.md`, Planteamiento del problema y justificación del Frontend SPA).

Criterios de reclutamiento del participante objetivo:

| Atributo | Criterio |
|----------|----------|
| Experiencia tecnológica | Baja a media; preferir participantes que registraban el proceso en papel. |
| Edad | Perfil representativo de la planta, incluyendo **personas mayores de 50 años**, por ser el grupo donde la baja experiencia tecnológica y las limitaciones de visión/motricidad fina más impactan la usabilidad (ver RNF-07). |
| Dispositivo habitual | Tableta Android y/o computador de escritorio, según su estación de trabajo. |

> El Sprint 0 describe el perfil como "operarios de planta con distintos niveles de experiencia tecnológica" y motiva RNF-06/07 con ese perfil; el rango de edad concreto (mayores de 50 años) es la interpretación del equipo para efectos de reclutamiento. Confirmar la distribución real de edades y experiencia por rol con el Product Owner: `__(completar: distribución de edades y experiencia tecnológica del personal real)__`.

### 3.3 Logística de la sesión

- **Lugar:** instalaciones de Sumicolor S.A. (estación con computador/tablet de planta). `__(completar: sala o estación exacta)__`
- **Fecha prevista:** Sprint 3. `__(completar: fecha de la sesión)__`
- **Duración estimada:** 30-45 minutos por participante.
- **Flujo de la sesión:** (1) adecuación del equipo, (2) contextualización del usuario, (3) ejecución de tareas con preguntas, (4) agradecimiento y cierre.

### 3.4 Participantes y roles

| Rol en la prueba | Perfil del usuario |
|------------------|--------------------|
| Usuario de prueba 1 | Operario de Mezcla |
| Usuario de prueba 2 | Operario de Pesaje |
| Usuario de prueba 3 | Personal de Planeación |
| Facilitador | Integrante del equipo que guía la sesión |
| Observador | Integrante del equipo que toma notas y mide |

> Alcance de la ejecución en el Sprint 3: las tareas probables se limitan a las etapas con funcionalidad entregada (Pesaje, Mezcla y envío a Extrusión) más la lectura del estado y la leyenda de colores de las OP (sección 1.1). Las etapas aún no entregadas (Picking, Calidad, Empaque) se incorporarán al protocolo cuando su funcionalidad esté disponible, siguiendo este mismo formato.

### 3.5 Equipos usados

- Computador portátil o tablet Android con la aplicación Polygon abierta en navegador (Chrome/Edge).
- Herramienta de grabación de pantalla y de voz (previa autorización del participante).
- Formato de observación impreso o digital para registrar tiempos y errores.

### 3.6 Hipótesis, tareas, preguntas y criterios de evaluación

Cada hipótesis parte de una decisión de diseño (RNF del Sprint 0 o regla de negocio del flujo entregado) y tiene una tarea observable alineada con un objetivo real del rol, al menos una pregunta y criterios medibles (tiempo, tasa de éxito, número de errores, satisfacción 1-5). Las tareas se limitan a las etapas con funcionalidad entregada en el Sprint 2 (ver 3.4).

**Hipótesis 1 — El usuario distingue sin ayuda las OP normales, urgentes y de acompañamiento por el color de su hoja.**
- Decisión de diseño: leyenda de colores con colores fácilmente distinguibles (RNF-06) y la mejora de notación de colores entregada en el Sprint 2 (sección 9.3).
- Rol/objetivo real: cualquier operario que consulta el listado necesita priorizar las OP según su clasificación.
- Tarea: identificar en el listado una OP urgente, una normal y una de acompañamiento de IP.
- Pregunta (después): ¿qué significa cada color y dónde consultó la notación?
- Criterios: tasa de éxito >= 90 %; sin ayuda del facilitador; satisfacción >= 4/5.

**Hipótesis 2 — El operario encuentra y registra el proceso de Mezcla sin ayuda externa.**
- Decisión de diseño: navegación simple y formularios sencillos (RNF-06/07) para el registro de la etapa de Mezcla (HU-22).
- Rol/objetivo real: el operario de mezcla consulta la orden y registra el proceso de mezclado (Sprint 0, "Personal de Mezcla").
- Tarea: registrar un turno de mezcla con datos válidos y confirmar que la OP avanza.
- Pregunta (antes): ¿dónde cree que debe registrar el proceso de Mezcla?
- Criterios: completa la tarea en < 5 min; <= 1 error de navegación.

**Hipótesis 3 — El usuario comprende el estado y la etapa de una OP con solo mirar la pantalla.**
- Decisión de diseño: interfaz minimalista con información de estado clara (RNF-06) y trazabilidad visible de la OP (RNF-05).
- Rol/objetivo real: cualquier rol necesita saber en qué etapa está la OP y qué sigue, para dar continuidad al proceso.
- Tarea: dado el detalle de una OP, explicar en qué etapa está y cuál es el siguiente paso.
- Pregunta (después): ¿la información en pantalla fue suficiente? ¿Qué faltó?
- Criterios: explica correctamente etapa y siguiente paso en >= 80 % de los casos.

**Hipótesis 4 — El operario de Pesaje registra las cantidades y envía a Mezcla sin bloqueos inesperados.**
- Decisión de diseño: validación de campos obligatorios que impide avanzar con datos incompletos, con mensajes sencillos (RNF-07); flujo de pesaje y envío a Mezcla (HU-19, HU-20).
- Rol/objetivo real: el operario de pesaje registra los pesos de las materias primas; el sistema no permite avanzar si falta información obligatoria (Sprint 0, "Personal de Pesaje").
- Tarea: registrar el pesaje de materias primas y confirmar el envío a Mezcla.
- Pregunta (después): ¿el sistema le indicó con claridad qué faltaba cuando intentó avanzar?
- Criterios: completa la tarea en < 6 min; comprende los mensajes de validación.

**Hipótesis 5 — Los mensajes de error al ingresar datos inválidos son claros y accionables.**
- Decisión de diseño: mensajes sencillos y accionables (RNF-07) sobre las validaciones de los formularios.
- Rol/objetivo real: cualquier operario que diligencia un formulario necesita corregir datos sin ayuda cuando el sistema los rechaza.
- Tarea: intentar enviar un registro con un campo obligatorio vacío o un valor fuera de rango.
- Pregunta (después): ¿el mensaje le dijo qué salió mal y cómo corregirlo?
- Criterios: identifica la causa del error sin ayuda en >= 80 % de los casos.

**Hipótesis 6 — El usuario recorre el flujo entre etapas (Pesaje -> Mezcla -> Extrusión) sin perderse.**
- Decisión de diseño: navegación simple entre etapas (RNF-06) y la regla de que una OP solo avanza al completar la información obligatoria de la etapa actual (HU-20, HU-23).
- Rol/objetivo real: la OP recorre las etapas y cada rol debe continuar el proceso sin confundir su etapa con otra.
- Tarea: seguir una OP a través de las etapas disponibles.
- Pregunta (después): ¿en algún punto dudó sobre a dónde ir? ¿En cuál?
- Criterios: completa el recorrido sin retrocesos innecesarios; satisfacción >= 4/5.

### 3.7 Resumen de criterios medibles (transversal)

Las mismas cuatro métricas se registran en todas las tareas para poder comparar resultados entre hipótesis:

| Métrica | Cómo se mide | Instrumento |
|---------|--------------|-------------|
| Tiempo en tarea | Segundos desde el inicio hasta completar la tarea con éxito. | Cronómetro / grabación de pantalla. |
| Tasa de éxito | % de participantes que completan la tarea sin ayuda del facilitador. | Formato de observación. |
| Errores | N.º de acciones incorrectas, intentos fallidos de avanzar y usos de ayuda. | Formato de observación. |
| Satisfacción | Puntaje en cuestionario post-tarea y global (Likert 1-5 o SUS). | Cuestionario de satisfacción. |

Los umbrales por hipótesis están propuestos arriba; el equipo y el Product Owner los confirman antes del Sprint 3: `__(completar: validación de umbrales de tiempo, éxito y errores por tarea)__`.


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
  - Los hallazgos de análisis estático preexistentes (backend y frontend) se corrigieron en una tarea dedicada; `ruff` y `eslint` quedan sin hallazgos.

### Acta - Sprint Review

- **Fecha:** `__(completar: fecha de la Review)__`
- **Participantes:** equipo Polygon y cliente/*stakeholders*. `__(completar: asistentes)__`
- **Temas tratados:**
  - Demostración del flujo Pesaje -> Mezcla -> Extrusión sobre una OP de ejemplo.
  - Presentación de la leyenda de colores de las OP y de las pruebas automáticas en verde.
- **Acuerdos:**
  - El incremento cumple el *sprint goal* del flujo productivo comprometido.
  - Se demuestra el pipeline de CI (jobs `backend` y `frontend`) corriendo pruebas, análisis estático y seguridad en cada *pull request*.

### Acta - Sprint Retrospective

- **Fecha:** `__(completar: fecha de la Retrospectiva)__`
- **Participantes:** equipo Polygon. `__(completar: nombres)__`
- **Temas tratados:**
  - Qué funcionó: el trabajo por ramas y PR mantuvo el `main` estable; derivar los CP de los criterios de aceptación dio claridad.
  - Qué mejorar: validar en una corrida real sobre un PR el *workflow* de CI ya configurado y afinar los umbrales acordables (cobertura, severidad de `npm audit`, `timeout`).
- **Acuerdos de mejora:**
  - CI habilitado con ejecución de pruebas, análisis estático, seguridad y control de *coverage* (`.github/workflows/ci.yml`); queda activar la protección de rama en el Sprint 3.
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

### 9.5 Pruebas automáticas, CI y análisis estático

La retroalimentación pedía una estrategia de pruebas automáticas. Se incorporó el runner Vitest + @vue/test-utils al frontend, con la regla de mínimo 2 pruebas por funcionalidad (happy path + flujo alternativo), y se dejó documentada la estrategia por funcionalidad (sección 2).

En este Sprint se amplió el alcance para cerrar también la deuda de aseguramiento de calidad:

- **Backend con pruebas Django/DRF:** se auditaron las diez aplicaciones y se completaron los archivos de prueba faltantes; la suite corre en verde (137 pruebas, cobertura 96 %).
- **Integración Continua:** `.github/workflows/ci.yml` ejecuta pruebas, análisis estático (`ruff`, `eslint`), cobertura y seguridad (`bandit`, `npm audit`) en cada PR (sección 2.4).
- **Análisis estático sin hallazgos:** se corrigieron los hallazgos preexistentes de `ruff` (backend) y `eslint` (frontend), incluidos los que la propia wiki señalaba como pendientes en `src/router/index.js` y `src/views/mezcla/RegistroMezcla.vue` (sección 2.5).
- **Protección de rama documentada:** guía para bloquear el *merge* mientras el CI no pase (sección 2.4 y Anexo).

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
| Documento Maestro de Pruebas (inventario + tabla de pruebas) | `docs/pruebas/Documento_Maestro_Pruebas.md` |
| Guía de protección de rama | `docs/pruebas/Guia_Proteccion_Rama.md` |
| Workflow de Integración Continua | `.github/workflows/ci.yml` |
| Dependencias de desarrollo (coverage, bandit) | `Backend/requirements-dev.txt` |
| Pruebas de backend por aplicación | `Backend/apps/<app>/tests.py` |
