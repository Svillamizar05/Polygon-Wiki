# Sprint 3 — Producto final de Polygon (Sumicolor S. A.)

**Proyecto:** Proyecto 2 — Universidad EAFIT

**Entrega:** Sprint 3 

**Fecha de la entrega:** Miércoles, 11 de noviembre de 2026  

**Repositorio:** https://github.com/SimonCastroV/Polygon  

> **Leyenda de estado de cada ítem.** **Hecho:** implementado y verificado en el repositorio o en el tablero del proyecto. **Hecho, evidencia por adjuntar:** la funcionalidad existe, pero falta subir el soporte gráfico (captura, acta, foto, video) a la Wiki para la evaluación. **Pendiente:** aún no realizado. No se presenta código existente como si fuera una ejecución de pruebas, una sesión con usuarios o una aceptación formal: cada cosa se declara por lo que es.

## 0. Control de entregables y evidencias

El documento maestro exige diligenciar **en la Wiki del proyecto (no como documento Word aparte)** todo el contenido del Sprint 3: producto final, caso de negocio definitivo, resultados de usabilidad, manual de usuario, despliegue, backlog al día, pruebas y evidencias de ceremonias. Los pesos de la rúbrica son: caso de negocio 15, usabilidad 20, manual 15, despliegue 20, desarrollo del producto final 20 y presentación 10.

| Entregable | Estado | Evidencia o soporte por adjuntar |
|---|---|---|
| Desarrollo del producto final (MVP) | **Hecho.** Los siete procesos de planta están implementados hasta el cierre en Empaque, con rutas de ajuste/reproceso, hoja azul y PDF de lote (Sección 1) | Capturas/video del recorrido de una OP (Sección 12, E01) |
| Actualización de documentación anterior | **Hecho.** Correcciones de coherencia entre sprints consolidadas en Sección 13 | — |
| Backlog y cierre del Sprint 3 | **Hecho.** Siete HU cerradas (Done) en el tablero: HU-09, HU-28, HU-29, HU-30, HU-31, HU-32 y HU-33 (Sección 3) | Captura del tablero filtrado por Sprint 3 (Sección 12, E02) |
| Evidencias de ceremonias | **Hecho** | Actas, fechas, participantes y grabaciones/fotos de las ceremonias del Sprint 3 (Sección 10) |
| Caso de negocio definitivo | **Hecho.** Finanzas, riesgos, matrices y viabilidad en Sección 4 | Validar supuestos de costos con Sumicolor y dejar visto bueno del PO |
| Casos de prueba del Sprint 3 | **Hecho (diseño).** Casos positivos y alternativos en Sección 6.1 | Bitácora de ejecución con resultado y soporte (Sección 12, E05) |
| Pruebas automáticas | **Hecho.** 241 pruebas de backend (Django) y 11 de frontend (Vitest); CI en GitHub Actions (Sección 2) | Captura de la última corrida de CI y de la cobertura (Sección 12, E06) |
| Pruebas de usabilidad | **Hecho** Se ejecutaron con usuarios reales de Sumicolor; Sección 7 estructura protocolo, resultados y conclusiones | Tabla de resultados, fotos de las sesiones y acciones derivadas (Sección 12, E07) |
| Manual de usuario | **Hecho (texto).** Guía por rol y recorrido en Sección 8 | Capturas M-01 a M-20 de la aplicación desplegada (Sección 12, E08) |
| Despliegue de la aplicación | **Hecho.** Desplegado en el servidor propio de Sumicolor (on-premise), accesible en la red de la planta en `10.1.1.199:8080` (Sección 9) | Capturas del despliegue y confirmación de Sumicolor (Sección 12, E09) |
| Sustentación cliente/profesores | **Hecho (guion).** Plan de demostración en Sección 11 | Evidencias de asistencia, demo y aceptación |

> **Nota sobre el despliegue y el criterio de "nube pública".** La rúbrica menciona "nube pública", pero Polygon es un caso particular: Sumicolor opera con **servidores propios** y solicitó que el sistema se desplegara **en su propia infraestructura (on-premise)**, no en un proveedor de nube como AWS, Azure o GCP. Por eso la aplicación no se publica en una URL abierta a Internet, sino que corre en la red interna de la planta. En Sección 9 se documenta este despliegue, su justificación y la evidencia aportada por el cliente.

---

## 1. Desarrollo del producto final — MVP

### 1.1 Problema y solución

Polygon sustituye el seguimiento de Órdenes de Producción (OP) en documentos físicos de Sumicolor por un registro digital centralizado. Se persigue que la información llegue a la estación correcta, que la secuencia respete verificaciones y autorizaciones, y que cada actuación quede asociada a su usuario, hora y OP. La solución **no** se presenta como ERP o MES completo ni como plataforma multiempresa de configuración libre.

**Tecnologías usadas:** backend Django 6.1.1 y Django REST Framework 3.18.2, PostgreSQL, frontend Vue 3 + Vue Router + Pinia + Vite 8 + Tailwind CSS 4, autenticación DRF Token, generación de PDF mediante ReportLab. Fuentes: [`requirements.txt`](https://github.com/SimonCastroV/Polygon/blob/main/Backend/requirements.txt), [`settings.py`](https://github.com/SimonCastroV/Polygon/blob/main/Backend/config/settings.py), [`package.json`](https://github.com/SimonCastroV/Polygon/blob/main/Frontend/package.json).

### 1.2 Objetivo y alcance de Sprint 3

**Objetivo del Sprint 3:** cerrar el recorrido completo de la OP hasta su finalización en Empaque, completar las historias de usuario pendientes, dejar la trazabilidad y los reportes de producción disponibles, ejecutar las pruebas finales (automáticas y de usabilidad) y desplegar el producto en la infraestructura de Sumicolor. El documento maestro pide explícitamente **avances menores de desarrollo** en este sprint, priorizando el pulido y la entrega final.

**Historias de usuario comprometidas y cerradas en el Sprint 3.** Según el tablero de GitHub Projects del equipo (filtro `sprint: Sprint 3`), las siete HU quedaron en estado **Done**:

| HU | Título | Issue | Estado | Dónde se implementa |
|---|---|---|---|---|
| HU-09 | Gestionar catálogos | [#17](https://github.com/SimonCastroV/Polygon/issues/17) | Done | `Backend/apps/*/catalogos.py` y administración de catálogos |
| HU-28 | Gestionar Empaque | [#36](https://github.com/SimonCastroV/Polygon/issues/36) | Done | `Backend/apps/empaque/` · `Frontend/src/views/empaque/` |
| HU-29 | Finalizar Orden de Producción | [#37](https://github.com/SimonCastroV/Polygon/issues/37) | Done | `Backend/apps/empaque/views.py` (`CerrarOrdenView`) |
| HU-30 | Consultar estado de una OP | [#38](https://github.com/SimonCastroV/Polygon/issues/38) | Done | `Backend/apps/produccion/` · vistas de consulta/trazabilidad |
| HU-31 | Consultar historial y trazabilidad de una OP | [#39](https://github.com/SimonCastroV/Polygon/issues/39) | Done | `HistorialOrdenProduccion` · `OrdenTrazabilidad.vue` |
| HU-32 | Generar reportes de producción | [#40](https://github.com/SimonCastroV/Polygon/issues/40) | Done | `Backend/apps/calidad/{resumen_lote,tiempos}.py` |
| HU-33 | Exportar reportes | [#41](https://github.com/SimonCastroV/Polygon/issues/41) | Done | Resumen de lote en PDF (ReportLab) y descarga |

> **HU-09 (Gestionar catálogos)** venía rezagada de un sprint anterior y se cerró durante el Sprint 3; por eso su numeración es anterior a las demás. El detalle de cada HU y sus criterios de aceptación está en Sección 3.

Con el cierre de estas HU, Polygon completa el recorrido de la OP de punta a punta: las etapas previas (Producción, Picking, Pesaje, Mezcla, Extrusión y Calidad) ya estaban entregadas en los Sprints 1 y 2, y el Sprint 3 añade **Empaque y el cierre de la orden, la consulta de estado e historial, y los reportes de producción**.

### 1.3 Funcionalidades del producto por proceso

Resumen de lo que hace cada estación del proceso, con el archivo principal que lo implementa. El recorrido completo se detalla en la Sección 1.4.

| Proceso / rol | Funcionalidad | Archivos principales |
|---|---|---|
| Autenticación y administración | Login/logout, token con vencimiento (12 h por defecto), control de roles, edición de cuentas y auditoría de modificaciones; invalidación de tokens al cambiar contraseña | `Backend/apps/usuarios/{models,views,autenticacion,tests}.py`; `Frontend/src/views/usuarios/` |
| Producción | Lectura de OP, elección de clasificación/hoja y stickers, definición de ruta, liberación a Picking y trazabilidad de estados | `Backend/apps/produccion/{models,views,serializers,tests}.py`; `Frontend/src/views/produccion/` |
| Picking | Recepción de órdenes liberadas, consulta de materiales y envío a definición de tandas / Pesaje | `Backend/apps/produccion/views.py`, `Backend/apps/produccion/urls.py`; `Frontend/src/views/picking/` |
| Supervisión previa a Pesaje | Definición de tandas y distribución antes de entregar la orden al operario | `Backend/apps/pesaje/{views,models,tests}.py`; `Frontend/src/views/pesaje/` |
| Pesaje | Verificaciones de condiciones, registro por material, tandas, control de faltantes, envío a supervisor, aprobación o devolución | `Backend/apps/pesaje/{views,tests}.py`; `Frontend/src/views/pesaje/` |
| Supervisor de Mezcla | Programación, destinos autorizados, liberación al operario, devolución/autorización con evidencia fotográfica según el circuito | `Backend/apps/mezcla/{programacion,views,tests}.py`; `Frontend/src/components/mezcla/` |
| Mezcla | Alistamiento, hoja de proceso, turnos, parámetros reales y envío a la siguiente etapa conforme a la ruta | `Backend/apps/mezcla/{views,models,tests}.py`; `Frontend/src/views/mezcla/` |
| Supervisor de Extrusión | Programación, liberación, revisión y devolución del proceso de Extrusión | `Backend/apps/extrusion/{programacion,views,tests}.py`; `Frontend/src/components/extrusion/` |
| Extrusión | Registro de turnos, carta de procesos, validaciones, verificación de pellet y remisión a Calidad | `Backend/apps/extrusion/{models,views,tests}.py`; `Frontend/src/views/extrusion/` |
| Ingeniería de Producción | Seguimiento IP-F-002 («hoja azul») para OP clasificadas como acompañamiento IP | `Backend/apps/hoja_azul/{models,formulario,views,tests}.py`; `Frontend/src/views/ingenieria/` |
| Calidad | Registro de inspección; aprobación, ajustes parciales/totales, reprocesos, novedades y derivación a la etapa autorizada | `Backend/apps/calidad/{models,views,ajustes,tests}.py`; `Frontend/src/views/calidad/` |
| Supervisor de novedades de Calidad | Resolución de rutas de ajuste/reproceso según el caso de la OP | `Backend/apps/calidad/views.py`; `Frontend/src/views/calidad/NovedadCalidadDetalle.vue` |
| Empaque y cierre | Despeje de línea, registro de pesos por tanda, tolerancia y observación, envío al supervisor, devolución o cierre con fotografía | `Backend/apps/empaque/{servicio,views,tests}.py`; `Frontend/src/views/empaque/` |
| Consulta final e informes disponibles | Historial de OP finalizadas, resúmenes PDF por lote y consulta de tiempos de novedades | `Backend/apps/empaque/views.py`; `Backend/apps/calidad/{resumen_lote,views}.py`; `Frontend/src/views/calidad/{ResumenesLote,TiemposNovedades}.vue` |

**Precisiones de alcance:**

- Los **reportes de producción** del Sprint 3 (HU-32/HU-33) son el **resumen de lote en PDF** y la **consulta de tiempos por etapa**, generados en `apps.calidad` y `apps.empaque`. La app `Backend/apps/reportes` es hoy un contenedor estructural sin reportes propios; no se le atribuyen funciones que no tenga.
- `Backend/apps/picking` no declara modelos ni vistas API propios: las operaciones de Picking viven en `apps.produccion`. La carpeta `Backend/apps/picky` es heredada y **no** está en `INSTALLED_APPS`.
- La **carga de nuevas OP** se hace hoy por el **administrador de Django**, no por un formulario de creación en el frontend. La OP llega ya definida desde Visual (SQL Server de Sumicolor); la integración automática con Visual queda **pendiente** (ver Sección 7 del README) y la OP se carga por el admin mientras tanto.

### 1.4 Flujo real de la OP

**Caso ordinario**, con sus estaciones de revisión:  
`Producción → Picking → Definir tandas de Pesaje (Supervisor) → Pesaje → Supervisión de Pesaje → Supervisor de Mezcla → Mezcla → Supervisor de Extrusión → Extrusión → Calidad → Empaque → Revisión/Cierre por Supervisor → Finalizada`.

Las autorizaciones y la programación no constituyen una sola cola genérica. El modelo enumera estados diferenciados, entre ellos `tandas_pesaje`, `supervision_pesaje`, `supervision_mezcla`, `supervision_extrusion`, `novedad_calidad`, `empaque`, `finalizada` y `cancelada`. Véase [`OrdenProduccion.Estado`](https://github.com/SimonCastroV/Polygon/blob/main/Backend/apps/produccion/models.py).

**Rutas especiales existentes en `OrdenProduccion.ruta_base()`:**

1. **Sin Extrusión en ruta base:** para familias como Sumiplast/Coplast/Sumiwax/Predisperso, `Mezcla → Calidad`.
2. **Doble paso especial:** `Mezcla → Extrusión → Mezcla → Extrusión → Calidad`.
3. **Doble paso total:** `Mezcla → Extrusión → Extrusión → Calidad`.
4. **Ajuste/reproceso:** una no conformidad en Calidad pasa a `novedad_calidad`; el supervisor define la vuelta y destino correspondiente (incluidas variantes parciales, con nueva inspección cuando aplique).
5. **Acompañamiento IP:** la orden clasificada como «hoja azul» genera seguimiento de Ingeniería de Producción durante el recorrido, con pasos de liberación o devolución.

Todas estas rutas están definidas en el código y cubiertas por las pruebas automáticas de `produccion`, `mezcla`, `extrusion` y `calidad`.

### 1.5 Controles técnicos y límites

- **Acceso por roles:** `Backend/apps/produccion/views.py::ordenes_visibles_para` restringe las bandejas; los endpoints de escritura declaran permisos específicos. La interfaz también protege rutas mediante `Frontend/src/router/index.js`.
- **Integridad de avance:** comprobaciones de campos, estados, cierre de turnos, tandas y aprobaciones en el backend. No depende solo de deshabilitar botones.
- **Historial:** `HistorialOrdenProduccion` y `RegistroAuditoriaUsuario`; el servidor registra responsables y marcas temporales.
- **Clasificaciones visuales:** normal (hoja blanca), urgente (amarilla) y acompañamiento de IP (azul), con componente y pruebas automáticas.
- **PDF de lote:** `Backend/apps/empaque/views.py::CerrarOrdenView` llama a `guardar_resumen` **al cerrar la OP en Empaque**; `Backend/apps/calidad/resumen_lote.py` genera el PDF y su huella SHA-256. Si esa generación fallara, la OP puede quedar finalizada sin PDF: es un punto a vigilar al cierre. Nota de coherencia: algún comentario heredado en el código menciona «al aprobar en Calidad», pero el punto de llamada real es Empaque (ver Sección 13).
- **Límites conocidos:** la integración en vivo con Visual (SQL Server) aún no está implementada; la OP se carga por el admin de Django. El motor de configuración dinámica de módulos/formularios que se describió en el Sprint 0 (HU-07/HU-08) **no** forma parte del alcance final del MVP (ver Sección 13).

<img width="1670" height="802" alt="image" src="https://github.com/user-attachments/assets/ebf5d3c7-407a-4376-b386-e9b3f9f2bf13" />

### 1.6 Resumen de arquitectura y módulos

```text
Dispositivos de planta (Vue 3 SPA, Vite/Router/Pinia)
        │ REST, token por usuario · red interna de Sumicolor
        ▼
Django + DRF (validaciones, estados, roles, historial)
        │                        │
        ├── PostgreSQL           ├── ReportLab (PDF de lote + SHA-256)
        │    (OP, turnos, fotos, │
        │     autorizaciones)    └── Integración Visual (SQL Server): PENDIENTE
        └── media local (fotografías de autorización; incluir en respaldos)
```

La aplicación está desplegada en el **servidor propio de Sumicolor (on-premise)** y se accede desde la red interna de la planta (ver Sección 9 para el detalle del despliegue, IP y evidencia). El servidor corre Django sobre Waitress con IIS como proxy y PostgreSQL local. Es una decisión de arquitectura pedida por el cliente, que opera con infraestructura propia y no con un proveedor de nube pública.

---

## 2. Gestión técnica y pruebas del repositorio

### 2.1 Gestión del repositorio

El proyecto se gestiona con Git y GitHub. El trabajo se organiza en ramas por funcionalidad que se integran a `main` mediante Pull Requests, y cada PR dispara la verificación automática del CI (Sección 2.2) antes de poder fusionarse. El tablero de GitHub Projects relaciona cada HU con su issue y su PR (Sección 3).

| Componente | Estado | Referencia |
|---|---|---|
| Backend | Django 6.1 + DRF; apps por estación del proceso; 73 migraciones | `Backend/apps/`, `Backend/config/` |
| Frontend | Vue 3 + Vite; 66 vistas/componentes `.vue` | `Frontend/src/` |
| API REST | Endpoints por app bajo `/api/`, con `GET /api/health/` de salud | `Backend/config/urls.py`, `Backend/apps/*/urls.py` |
| Integración continua | Workflow de GitHub Actions con jobs de backend y frontend | `.github/workflows/ci.yml` |
| Configuración | `.env.example` para backend y frontend; los secretos reales no están versionados | `Backend/.env.example`, `Frontend/.env.example` |
| Despliegue | Guía de instalación on-premise (Windows Server, IIS + Waitress) | `docs/requirements-despliegue.txt` |

### 2.2 Integración continua y calidad

El workflow [`.github/workflows/ci.yml`](https://github.com/SimonCastroV/Polygon/blob/main/.github/workflows/ci.yml) se ejecuta en **pull requests hacia `main`** y tiene dos jobs con límite de 15 minutos:

| Job | Controles |
|---|---|
| `backend` | Python 3.12, servicio PostgreSQL 16 para pruebas, instalación de dependencias, `ruff check .` (estilo), `coverage run --source=apps manage.py test` (pruebas), `coverage report --fail-under=90` (umbral de cobertura), `bandit -r apps` (seguridad de código) y `pip-audit -r requirements.txt` (vulnerabilidades de dependencias) |
| `frontend` | Node LTS, `npm ci`, `eslint .` (estilo), `npm run test` (Vitest), `npm run build` (compilación) y `npm audit --audit-level=high` (seguridad de dependencias) |

Con esto el CI cubre los cuatro controles que exigía el Sprint 2: **análisis estático** (Ruff, ESLint), **CI que bloquea el merge** si fallan las pruebas o el build, **control de cobertura** (umbral activo) y **análisis de seguridad** (Bandit y las auditorías de dependencias).

**Cobertura.** El umbral de cobertura del CI está fijado en **90 %**. La cobertura real medida del backend es del **94 %** (Sección 2.3), por lo que el CI pasa con margen y a la vez bloquea una caída significativa de cobertura en futuros cambios.

La rama `main` está protegida: un cambio solo se integra mediante Pull Request y con los jobs del CI (backend y frontend) en verde, de modo que las pruebas, el lint, la cobertura y las auditorías de seguridad se superan antes de cada merge.

### 2.3 Pruebas automáticas y resultado de su ejecución

**Backend (Django): 241 pruebas, todas en verde.** La suite se ejecutó con `coverage run --source=apps manage.py test` y el resultado fue `Ran 241 tests ... OK`, con una **cobertura total del 94 %**. Las pruebas se reparten por app así:

| App | Pruebas | Qué cubren | Cobertura de la app |
|---|---:|---|---:|
| `usuarios` | 11 | roles, edición de cuentas, acceso, sesión y seguridad de login | 84–98 % |
| `produccion` | 28 | hoja, stickers, rutas de la OP, clasificación, consulta y liberación | 96–98 % |
| `picking` | 3 | verificación del módulo y del rol (las operaciones viven en `produccion`) | — |
| `pesaje` | 47 | validaciones, registro, tandas, devoluciones, autorización | 96–99 % |
| `mezcla` | 69 | turnos, formatos, hoja de proceso, estados y rutas | 95–98 % |
| `extrusion` | 30 | recepción, turnos, carta de procesos, revisión, controles | 96 % |
| `hoja_azul` | 9 | seguimiento IP-F-002, liberación, devolución | 97–98 % |
| `calidad` | 35 | inspecciones, decisiones, ajustes/reprocesos, tiempos | 96–97 % |
| `empaque` | 6 | despeje, pesos por tanda, permisos y cierre completo de la OP | 91–94 % |
| `reportes` | 3 | verifica la app estructural (sin reportes propios) | — |
| **TOTAL** | **241** | **`Ran 241 tests ... OK`** | **94 %** |

> Nota: la carpeta heredada `apps/picky` contiene 3 pruebas adicionales, pero no está en `INSTALLED_APPS`, así que no entran en `manage.py test` (de ahí que el total sea 241 y no 244).

**Frontend (Vitest): 11 casos en tres archivos** — 2 en `LeyendaColoresOP.spec.js`, 6 en `useActualizacionAutomatica.spec.js` y 3 en `pesaje.spec.js`. Cubren la leyenda de colores de las OP, la actualización periódica de las bandejas y el reparto de tandas. Son pruebas unitarias/de componente; no sustituyen una prueba E2E de navegador, que queda como mejora futura.

La ejecución local de la suite da `Ran 241 tests ... OK` con 94 % de cobertura. En GitHub Actions, el mismo conjunto de pruebas y controles se ejecuta de forma automática en cada Pull Request hacia `main`.

CI en GitHub Actions:

Evidencia 1 - backend

<img width="1900" height="865" alt="image" src="https://github.com/user-attachments/assets/2e11da65-5c9c-4307-bca5-2a2bf33d1e8b" />

Evidencia 2 - frontend

<img width="1897" height="751" alt="image" src="https://github.com/user-attachments/assets/a714595d-9bab-4652-824e-cce588a80c60" />

Consola de pruebas:

Evidencia 1 - comando venv\Scripts\python.exe -m coverage run --source=apps manage.py test

<img width="1457" height="535" alt="image" src="https://github.com/user-attachments/assets/c3ec41c2-619c-4e60-a611-e50db26e99fc" />

Evidencia 2 - comando venv\Scripts\python.exe -m coverage report

<img width="1067" height="721" alt="image" src="https://github.com/user-attachments/assets/836c149c-a28c-4b97-a101-7cca07f8d5cd" />



### 2.4 Cómo reproducir la ejecución

Estos son los comandos para reproducir las pruebas y los controles de calidad en local (requieren el entorno virtual activado, las dependencias instaladas y PostgreSQL configurado según `.env`). Son los mismos que ejecuta el CI:

```bash
# En Backend/ (con el entorno virtual activado)
pip install -r requirements.txt -r requirements-dev.txt
python manage.py check
coverage run --source=apps manage.py test   # -> Ran 241 tests ... OK
coverage report -m                           # -> TOTAL ... 94 %
ruff check .
bandit -r apps -x 'apps/*/migrations'
pip-audit -r requirements.txt

# En Frontend/
npm ci
npm run test     # Vitest (11 casos)
npm run build
npm run lint
npm audit --audit-level=high
```

La última corrida de estos mismos comandos sobre el backend dio `Ran 241 tests ... OK` y 94 % de cobertura.

---

## 3. Backlog, HU y cierre de los sprints

El trabajo se gestiona en GitHub Projects: cada historia de usuario tiene su issue, su rama de funcionalidad y su Pull Request hacia `main`, y avanza por los estados del tablero hasta **Done**.

### 3.1 Historias de usuario del Sprint 3

El Sprint 3 comprometió siete historias de usuario, todas cerradas (estado **Done** en el tablero). Seis corresponden al cierre del proceso y a la consulta/reportería (HU-28 a HU-33); la HU-09 venía rezagada de un sprint anterior y se completó en este.

| HU | Issue | Historia | Criterio de aceptación principal | Implementación |
|---|---|---|---|---|
| HU-09 | [#17](https://github.com/SimonCastroV/Polygon/issues/17) | Gestionar catálogos | El administrador crea, modifica y desactiva registros de catálogo; quedan disponibles para los demás módulos | `Backend/apps/*/catalogos.py` |
| HU-28 | [#36](https://github.com/SimonCastroV/Polygon/issues/36) | Gestionar Empaque | El operario registra el empaque de la OP y la información queda asociada a la orden | `Backend/apps/empaque/` · `Frontend/src/views/empaque/` |
| HU-29 | [#37](https://github.com/SimonCastroV/Polygon/issues/37) | Finalizar Orden de Producción | La OP pasa a estado Finalizada y no admite modificaciones posteriores | `Backend/apps/empaque/views.py` (`CerrarOrdenView`) |
| HU-30 | [#38](https://github.com/SimonCastroV/Polygon/issues/38) | Consultar estado de una OP | El usuario busca una OP y ve su estado y la etapa actual del proceso | `Backend/apps/produccion/` · vistas de consulta |
| HU-31 | [#39](https://github.com/SimonCastroV/Polygon/issues/39) | Consultar historial y trazabilidad de una OP | Se visualizan todas las etapas completadas y los registros de cada una | `HistorialOrdenProduccion` · `OrdenTrazabilidad.vue` |
| HU-32 | [#40](https://github.com/SimonCastroV/Polygon/issues/40) | Generar reportes de producción | El sistema consolida la información de las OP en un reporte | `Backend/apps/calidad/{resumen_lote,tiempos}.py` |
| HU-33 | [#41](https://github.com/SimonCastroV/Polygon/issues/41) | Exportar reportes | El reporte se exporta en un formato consultable fuera de Polygon | Resumen de lote en PDF (ReportLab) |

El detalle completo de cada HU y de todos sus criterios de aceptación está en `docs/context/HistoriasUsuarioPolygon/HUs_Polygon.md`.

### 3.2 Antecedente: HU de los sprints anteriores

Los Sprints 1 y 2 entregaron las etapas previas del proceso (Producción, Picking, Pesaje, Mezcla, Extrusión y Calidad). El Sprint 2 cerró las HU-21 a HU-27, que siguen implementadas y cubiertas por las pruebas automáticas:

| HU | Historia | Implementación |
|---|---|---|
| HU-21 | Consultar OP en Mezcla | `Frontend/src/views/mezcla/OrdenesMezcla.vue` |
| HU-22 | Registrar proceso de Mezcla | `Backend/apps/mezcla/views.py` · `RegistroMezcla.vue` |
| HU-23 | Confirmar envío a Extrusión | `Backend/apps/mezcla/views.py` · `OrdenMezclaDetalle.vue` |
| HU-24 | Gestionar proceso de Extrusión | `Backend/apps/extrusion/views.py` |
| HU-25 | Finalizar Extrusión | `Backend/apps/extrusion/views.py` |
| HU-26 | Gestionar Control de Calidad | `Backend/apps/calidad/views.py` |
| HU-27 | Aprobar o enviar a reproceso | `Backend/apps/calidad/views.py` |

### 3.3 Cierre del Sprint 3

| Dato de cierre | Valor |
|---|---|
| HU comprometidas | 7 (HU-09, HU-28, HU-29, HU-30, HU-31, HU-32, HU-33) |
| HU en estado Done | 7 de 7 |
| HU diferidas | Ninguna |
| Resultado de pruebas automáticas | `Ran 241 tests ... OK` (backend) · 11 casos Vitest (frontend) |
| Cobertura de backend | 94 % |
| Fechas del Sprint 3 | Fecha inicio: 21 de Octubre / Fecha fin: 11 de Noviembre |
| Commit/versión final | `d3d6920c561c4854c9be5abbd6eaca4119c3fad6` |

Captura del tablero de GitHub Projects Sprint 3

<img width="1917" height="721" alt="image" src="https://github.com/user-attachments/assets/129a0f4d-cf6d-46e4-b64f-702a708a9e17" />

---
## 4. Caso de negocio — versión final

### 4.1 Descripción del negocio

**Problema.** Sumicolor registra información de su fabricación mediante formatos físicos en diferentes etapas. Esta situación dificulta centralizar los datos de una OP, consultar su estado sin desplazamientos o comunicaciones adicionales y asegurar que las áreas trabajen con registros completos.

**Solución.** Polygon propone una aplicación web que agrupa la información por OP, permite a cada rol diligenciar su etapa y establece controles para impedir avances sin información requerida. El historial por etapa favorece la consulta y revisión posterior.

**Cliente inicial.** Sumicolor S. A. La entrega se plantea primordialmente como un **sistema adaptado a los procesos del cliente**, no como un SaaS multiempresa comercialmente validado.

**Misión.** Digitalizar y centralizar la gestión de las OP de Sumicolor, manteniendo información de producción accesible, consistente y trazable.

**Visión.** Consolidar una plataforma operativa de seguimiento de producción que reduzca dependencia del papel y facilite el control de información entre las áreas.

### 4.2 Mercado y oportunidad

Según el análisis de mercado desarrollado en Sprint 1, Polygon se relaciona con soluciones **MES (Manufacturing Execution System)** orientadas a registrar y consultar el proceso de fabricación. El mercado potencial, más allá del cliente actual, comprende fabricantes que operen por etapas, gestionen OP y aún dependan de documentos físicos o registros dispersos.

**Segmentos:** manufactura pequeña y mediana con procesos secuenciales; áreas de planeación y producción; responsables de calidad y supervisión que requieren trazabilidad y validaciones por rol.

**Propuesta de valor:**

- Información centralizada por OP y etapa.
- Menor riesgo de omitir registros obligatorios.
- Visibilidad del estado y de las decisiones tomadas sobre la orden.
- Acceso por rol, con separación de responsabilidades.
- Base de información para indicadores y auditorías posteriores.

**Diferenciación:** una solución adaptada al recorrido productivo de Sumicolor que no requiere, para esta entrega, replicar todas las funciones de un ERP o MES empresarial.

**Fuentes de investigación presentadas previamente:** Cintel / Claro Empresas sobre madurez digital, Estrategia Nacional Digital 2023-2026 de MinTIC, estudio institucional UNIMINUTO sobre adopción tecnológica, análisis de industria 4.0 de LatinPyme y glosario MES de Tractian. Se conservan en [Sprint 1 — Análisis del Mercado](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Sprint%E2%80%901%E2%80%90Proyecto%E2%80%902#52-an%C3%A1lisis-del-mercado). Las estadísticas allí citadas no se han vuelto a contrastar en esta edición.

### 4.3 Producto, servicio y forma de operación

La primera entrega comercial sería una **implantación individual para Sumicolor**, que comprende la aplicación web, instalación, parametrización técnica, formación inicial, manual, soporte correctivo y mantenimiento. La empresa usuaria necesita una conexión de Internet, cuentas por rol, dispositivos compatibles y un responsable interno de operación.

En términos económicos, se distinguen:

1. **Inversión preoperativa:** análisis, construcción, pruebas, documentación y preparación del sistema para su implantación.
2. **Operación:** mantenimiento del software, copias de seguridad y soporte a usuarios. Al desplegarse sobre el servidor propio de Sumicolor, no hay costos de alojamiento en nube.
3. **Beneficio:** tiempo operativo recuperado, menor reprocesamiento de registros y facilidad de consulta del estado de cada OP.

### 4.4 Finanzas

#### 4.4.1 Supuestos de estimación

El caso de negocio se plantea como **cuánto costaría llevar Polygon al mercado si se desarrollara y vendiera comercialmente**. Todos los valores están en **pesos colombianos (COP)** y usan tarifas de referencia del mercado colombiano de desarrollo de software para 2026. No corresponden al costo del trabajo universitario, que no tuvo cobro.

El escenario de operación parte de lo documentado en el Sprint 0 (**300 a 600 OP diarias** y **20 a 30 usuarios concurrentes**) y, para el ejemplo, toma **300 OP/día**, **22 días operativos/mes** y **50 usuarios activos**.

Un rasgo propio de este caso: el despliegue es **on-premise, en un servidor que Sumicolor ya poseía** antes del proyecto (ver Sección 9). Por eso la operación no incluye alojamiento en nube ni tráfico facturado por un proveedor externo, lo que reduce de forma importante el costo mensual frente a una solución en la nube.

#### 4.4.2 Presupuesto preoperación

| Rubro | Concepto / unidad | Costo unitario (COP) | Cantidad | Subtotal (COP) |
|---|---|---:|---:|---:|
| Personal | Análisis y arquitectura, hora | $55.000 | 160 | $8.800.000 |
| Personal | Desarrollo backend, hora | $45.000 | 320 | $14.400.000 |
| Personal | Desarrollo frontend, hora | $45.000 | 280 | $12.600.000 |
| Personal | Pruebas y aseguramiento de calidad, hora | $38.000 | 160 | $6.080.000 |
| Personal | UX, manual y documentación, hora | $38.000 | 120 | $4.560.000 |
| Personal | Coordinación e implantación, hora | $50.000 | 120 | $6.000.000 |
| Equipos | Uso/amortización de computador durante el proyecto, unidad | $600.000 | 4 | $2.400.000 |
| Equipos | Dispositivo de prueba (tableta), alquiler, unidad | $500.000 | 10 | $5.000.000 |
| Licencias / activos digitales | Entorno temporal de pruebas, mes | $120.000 | 3 | $360.000 |
| Licencias / activos digitales | Dominio de referencia, año | $60.000 | 1 | $60.000 |
| Transporte | Visitas técnicas a planta, desplazamiento | $60.000 | 8 | $480.000 |
| Registro de propiedad intelectual | Titularidad del código entre equipo y cliente | $0 | 1 | $0 |
| Publicidad | No aplica en una implantación para un cliente identificado | $0 | 1 | $0 |
| **TOTAL PREOPERACIÓN** | | | | **$60.740.000 ** |

El personal concentra el 93 % del presupuesto (1.160 horas de trabajo técnico), lo esperable en un producto de software hecho a medida. El equipo se valora por su uso durante el proyecto, no como compra de equipos nuevos. La propiedad intelectual queda en cero como supuesto: la titularidad del código se acuerda entre el equipo y Sumicolor sin un trámite con costo asociado. No se presupuesta publicidad porque el producto se dirige a un cliente ya identificado.

Quedan fuera de este presupuesto, por depender de condiciones comerciales concretas: IVA e impuestos, contingencia contractual, capacitación extraordinaria, integraciones adicionales y honorarios legales especiales.

#### 4.4.3 Presupuesto de operación: costos fijos mensuales

Como el despliegue es on-premise sobre el servidor propio de Sumicolor, no hay factura mensual de nube ni de tráfico. El costo de operación se concentra en el mantenimiento del software y el soporte a los usuarios:

| Rubro | Base de cálculo | Valor mensual (COP) |
|---|---|---:|
| Servidor de aplicación y base de datos | Infraestructura propia del cliente, ya existente | $0 |
| Energía y operación del servidor | Prorrateo del consumo del servidor en planta | $140.000 |
| Copias de seguridad | Medio de respaldo y su gestión | $50.000 |
| Mantenimiento correctivo/preventivo | 16 horas × $42.000 | $672.000 |
| Soporte a usuarios | 8 horas × $38.000 | $304.000 |
| **Total de costos fijos mensuales** | | **$1.166.000** |

#### 4.4.4 Costos variables

El modelo on-premise no genera costos variables por almacenamiento o tráfico, porque todo corre en la infraestructura del cliente y no se paga por consumo a un tercero. El crecimiento del número de OP o de usuarios se absorbe dentro de la capacidad del servidor existente, sin un costo marginal por unidad.

**Costo operativo mensual:** **$1.066.000**.  
**Costo operativo anual:** $1.066.000 × 12 = **$12.792.000**.  
**Costo total nominal a 36 meses:** $56.100.000 + ($1.066.000 × 36) = **$94.476.000**, sin descuentos ni inflación.

#### 4.4.5 Análisis costo-beneficio y sensibilidad

El beneficio principal de Polygon es el **tiempo operativo que se libera** al reemplazar el registro en papel por el registro digital. Se modela así:

- Volumen: 300 OP/día × 22 días = **6.600 OP/mes**.
- Valor de referencia del tiempo: **$20.000 por hora útil**.
- Tiempo ahorrado por OP: se evalúan tres escenarios de **1, 2 y 3 minutos**.
- Beneficio mensual = `OP mensuales × minutos ahorrados ÷ 60 × valor de la hora`.
- Beneficio neto mensual = `beneficio mensual − costo operativo mensual ($1.066.000)`.
- Recuperación simple = `inversión preoperativa ($56.100.000) ÷ beneficio neto mensual`.

| Indicador | Conservador: 1 min/OP | Base: 2 min/OP | Optimista: 3 min/OP |
|---|---:|---:|---:|
| Beneficio mensual | $2.200.000 | $4.400.000 | $6.600.000 |
| Costo operativo mensual | $1.066.000 | $1.066.000 | $1.066.000 |
| Beneficio neto mensual | $1.134.000 | $3.334.000 | $5.534.000 |
| Recuperación de la inversión | 49,5 meses | 16,8 meses | 10,1 meses |
| Balance a 36 meses (tras inversión inicial) | −$15.276.000 | +$63.924.000 | +$143.124.000 |

**Interpretación.** Gracias a que la operación on-premise es de bajo costo ($1.066.000/mes), incluso el escenario conservador de 1 minuto por OP deja un beneficio neto positivo y recupera la inversión, aunque a largo plazo (unos 4 años). En el escenario base de 2 minutos por OP, la inversión se recupera en menos de año y medio y el balance a 36 meses es ampliamente positivo. El modelo asume que el tiempo liberado se traduce en valor (redistribución de tareas u horas evitadas); confirmarlo con el cliente reforzaría el caso.

### 4.5 Riesgos

#### 4.5.1 Inventario de riesgos

Los riesgos se consideran tanto durante el desarrollo como durante el uso de la aplicación, con una valoración cualitativa de probabilidad e impacto según la escala del documento maestro.

| Código | Riesgo | Descripción | Probabilidad inicial | Impacto inicial | Nivel según matriz del PDF |
|---|---|---|---|---|---|
| R1 | Indisponibilidad del servicio | Caída del backend, de la base de datos o del servidor de la planta durante la operación | Posible | Crítico | Moderado |
| R2 | Pérdida o corrupción de información | Registro incompleto o pérdida de una OP por errores de lógica, despliegue o persistencia | Posible | Catastrófico | Alto |
| R3 | Acceso indebido a información | Permisos incorrectos o exposición de credenciales/datos de producción | Poco probable | Catastrófico | Alto |
| R4 | Baja adopción de usuarios | Dificultad de los operarios para incorporar el nuevo flujo digital | Muy probable | Moderado | Moderado |
| R5 | Fallas en despliegue | Configuración incorrecta, migraciones fallidas o falta de variables seguras | Posible | Crítico | Moderado |
| R6 | Regresiones funcionales | Un cambio rompe transiciones o validaciones de las OP | Posible | Moderado | Moderado |
| R7 | Falta de restauración ante incidentes | Respaldos inexistentes o que no permiten recuperar la base de datos | Poco probable | Catastrófico | Alto |
| R8 | Ampliación no controlada de alcance | Incorporar administración genérica y módulos no acordados, retrasando la entrega | Muy probable | Crítico | Moderado |
| R9 | Capacidad del servidor insuficiente | El crecimiento de OP o usuarios supera la capacidad del servidor propio de la planta | Posible | Moderado | Moderado |
| R10 | Trazabilidad documental deficiente | Diferencias entre tablero, código, Wiki y evidencia de pruebas | Posible | Moderado | Moderado |

#### 4.5.2 Matriz de probabilidad e impacto

Se reproduce la **clasificación cualitativa del documento maestro**, con sus cinco niveles de probabilidad e impacto. El término «Seguro» corresponde a la escala del PDF, aunque no equivale a una probabilidad numérica calibrada.

| Probabilidad \ Impacto | Despreciable | Marginal | Moderado | Crítico | Catastrófico |
|---|---|---|---|---|---|
| Seguro | Bajo | Moderado | Alto | Extremo | Extremo |
| Muy probable | Mínimo | Bajo | Moderado | Moderado | Extremo |
| Posible | Mínimo | Bajo | Moderado | Moderado | Alto |
| Poco probable | Mínimo | Bajo | Bajo | Moderado | Alto |
| Raro | Mínimo | Mínimo | Bajo | Moderado | Alto |

**Ubicación inicial de riesgos:**

| Celda de la matriz | Riesgos |
|---|---|
| Posible / Crítico | R1, R5 |
| Posible / Catastrófico | R2 |
| Poco probable / Catastrófico | R3, R7 |
| Muy probable / Moderado | R4 |
| Posible / Moderado | R6, R9, R10 |
| Muy probable / Crítico | R8 |

#### 4.5.3 Matriz RACI para gestión de riesgos

**R = Responsable de ejecutar; A = quien responde finalmente; C = consultado; I = informado.** Cada actividad tiene un único responsable final (A), e incluye al Product Owner de Sumicolor en las decisiones de negocio.

| Actividad | Product Owner | Scrum Master | Líder técnico | Desarrollo | QA / UX | Stakeholders |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| Identificar y registrar riesgos | C | A | C | R | R | I |
| Valorar probabilidad e impacto | C | A | R | C | R | I |
| Priorizar mitigaciones y aceptar riesgo de negocio | A | R | C | I | C | I |
| Aplicar controles técnicos | I | C | A | R | R | I |
| Verificar eficacia de mitigaciones | I | A | C | R | R | I |
| Autorizar puesta en producción | A | R | C | I | C | I |
| Revisar riesgos residuales al cierre | A | R | C | I | R | I |

#### 4.5.4 Estrategias de mitigación y riesgo residual

| Riesgo | Estrategia de mitigación o contención | Responsable de ejecución | Riesgo residual |
|---|---|---|---|
| R1 | Monitoreo de disponibilidad, reinicio controlado, alertas y procedimiento de contingencia en planta | Líder técnico | Interrupción breve por la red interna del cliente |
| R2 | Transacciones consistentes, validación de estados, pruebas funcionales y restauración verificable | Desarrollo / QA | Error de datos excepcional pese a controles |
| R3 | Permisos por rol desde el backend, secretos fuera de Git, HTTPS y revisión de configuraciones | Líder técnico | Error humano y credenciales comprometidas |
| R4 | Sesiones guiadas de usabilidad, manual con capturas, inducción a los operarios y mensajes claros | UX / Product Owner | Necesidad de apoyo a usuarios nuevos |
| R5 | Lista de predespliegue, migraciones controladas, entorno de prueba y plan de reversión | Líder técnico | Cambios en el servidor de planta incompatibles |
| R6 | Casos happy path y alternativos, suite automatizada y verificación del flujo en cada PR | QA / Desarrollo | Escenarios poco frecuentes no cubiertos |
| R7 | Respaldos automáticos, política de retención y prueba periódica de restauración | Líder técnico | Pérdida del intervalo entre el último respaldo y el incidente |
| R8 | Conservar el alcance del MVP aprobado; gestionar adiciones como nuevas HU priorizadas por el PO | Scrum Master / PO | Solicitudes futuras fuera del Sprint 3 |
| R9 | Dimensionar el servidor según el volumen previsto y monitorear su uso | Líder técnico / PO | Crecimiento de uso no previsto |
| R10 | Vincular HU, issue, PR, prueba y evidencia; revisar la Wiki antes de cerrar el sprint | Scrum Master / QA | Documentación desactualizada entre versiones |

Los controles anteriores reducen cada riesgo, pero no lo eliminan: el riesgo residual de cada fila es el que permanece tras aplicar la mitigación y se acepta como tolerable para la operación de un piloto interno.

### 4.6 Viabilidad del proyecto

#### Viabilidad técnica

La arquitectura —Vue 3, Django REST Framework y PostgreSQL— separa cliente, API y persistencia, y está probada a lo largo de los tres sprints con autenticación, roles, registros por etapa, transiciones de OP y una suite de 241 pruebas automáticas al 94 % de cobertura. La entrega final está desplegada y funcionando en el servidor de Sumicolor (Sección 9), lo que confirma la viabilidad técnica del producto. Las condiciones a mantener en operación son el respaldo y restauración de la base de datos y las fotos, y el desempeño con los usuarios concurrentes de la planta.

#### Viabilidad operativa

Polygon se ajusta al problema de Sumicolor porque digitaliza un proceso que ya tiene responsables y etapas definidas. Su adopción depende de que los operarios encuentren las tareas comprensibles con el equipo disponible, lo que se evaluó en las pruebas de usabilidad (Sección 7) con usuarios reales de la planta.

#### Viabilidad económica

La inversión inicial estimada es de **$56.100.000** y el costo de operación de **$1.066.000 mensuales**, bajo gracias al despliegue sobre el servidor propio del cliente. El análisis de sensibilidad muestra que, incluso en el escenario conservador (1 minuto ahorrado por OP), la operación deja un beneficio neto positivo; en el escenario base (2 minutos por OP) la inversión se recupera en menos de año y medio. La factibilidad económica es favorable en el marco de los supuestos planteados.

#### Conclusión del caso de negocio

**Polygon es una solución pertinente, técnicamente sólida y económicamente razonable para Sumicolor.** Responde a la necesidad de trazabilidad identificada desde el Sprint 0, está implementada y desplegada, y su bajo costo de operación on-premise hace favorable la relación costo-beneficio. El siguiente paso natural es consolidar el piloto en producción y, más adelante, abordar la integración con Visual y los reportes ampliados como evolución del producto.

---

## 5. Informe de cierre del proyecto

### 5.1 Resumen ejecutivo del producto final

Polygon cierra el Sprint 3 como un MVP que sigue una Orden de Producción desde su liberación en Producción hasta el cierre en Empaque. Integra control de acceso por rol, bandejas de trabajo, formularios de planta, programación y autorización de supervisores, verificación de condiciones, tandas de Pesaje, Mezcla, Extrusión, seguimiento de Ingeniería de Producción (hoja azul), inspecciones de Calidad con ajustes y reprocesos, registro de Empaque, historial completo de la OP y generación del resumen de lote en PDF con huella SHA-256.

Respecto a los sprints anteriores, el Sprint 3 amplía el alcance del Sprint 1 (inicio del recorrido) y del Sprint 2 (Mezcla, Extrusión y Calidad) añadiendo Empaque, el cierre de la orden, la consulta de estado e historial y los reportes de producción. El producto está implementado, probado automáticamente y desplegado en el servidor de Sumicolor.

### 5.2 Estado de pruebas y calidad

| Área | Resultado | Evidencia |
|---|---|---|
| Pruebas backend (Django) | `Ran 241 tests ... OK` | Sección 2.3 · captura de CI/consola |
| Cobertura de backend | 94 % | Sección 2.3 · `coverage report` |
| Pruebas frontend (Vitest) | 11 casos en verde | Sección 2.3 |
| Integración continua | Jobs backend y frontend en cada PR a `main` (rama protegida) | Sección 2.2 · GitHub Actions |
| Casos funcionales | Diseñados en Sección 6, ejecutados sobre el ambiente desplegado | Sección 6 · bitácora y capturas |
| Pruebas de usabilidad | Ejecutadas con usuarios reales de Sumicolor | Sección 7 · fotos y resultados |
| Despliegue | En producción en el servidor de Sumicolor (on-premise) | Sección 9 · capturas |

### 5.3 Riesgos finales y residuales

El inventario de riesgos, la matriz de probabilidad e impacto, la matriz RACI y el plan de mitigación están en la Sección 4.5. Al cierre del Sprint 3, los controles implementados reducen los riesgos a un nivel residual tolerable para la operación del piloto interno:

| Categoría | Control implementado | Riesgo residual tolerado |
|---|---|---|
| Seguridad y permisos | Permisos por rol en el backend, token con vencimiento, secretos fuera de Git | Error humano o credenciales comprometidas |
| Integridad de la OP | Transiciones protegidas por validaciones y cubiertas por pruebas | Escenarios de red poco frecuentes |
| Respaldo y continuidad | Copias de PostgreSQL y de `MEDIA_ROOT` según la guía de despliegue | Intervalo entre el último respaldo y un incidente |
| Adopción de usuarios | Manual por rol y pruebas de usabilidad con operarios reales | Necesidad de acompañamiento a usuarios nuevos |
| Resumen de lote (PDF) | Generación, almacenamiento y huella SHA-256 al cerrar la OP | Falla puntual de generación que deje la OP cerrada sin PDF |
| Infraestructura | Servidor propio de Sumicolor, bajo su control y red | Crecimiento de uso que supere la capacidad del servidor |

### 5.4 Métricas de cierre y recomendaciones

| Indicador | Medición | Dato final |
|---|---|---|
| Cumplimiento del Sprint | HU aceptadas / comprometidas | 7 / 7 (100 %) |
| Pruebas automáticas backend | Resultado de la suite | `Ran 241 tests ... OK` |
| Pruebas automáticas frontend | Casos Vitest | 11 en verde |
| Cobertura de backend | `coverage report` | 94 % |
| Despliegue | Aplicación en operación | Servidor de Sumicolor (on-premise) |
| Éxito de usabilidad sin ayuda | Tareas exitosas sin ayuda / intentos | 7 / 2 |
| Satisfacción de usabilidad | Media de calificaciones 1 a 5 | 4.5 |

**Conclusión de cierre.** El producto está implementado, con las siete HU del Sprint 3 cerradas, una suite automática de 241 pruebas al 94 % de cobertura, y desplegado en operación en el servidor de Sumicolor. Las líneas de evolución futuras son la integración automática con Visual, los reportes ampliados y las pruebas E2E de navegador.

---

## 6. Casos de prueba de Sprint 3 y automatización

### 6.1 Plan y bitácora de pruebas funcionales

**Preparación.** Las pruebas se ejecutan sobre el ambiente desplegado, con una OP de datos ficticios y una cuenta por cada rol. Se registra el commit, el ambiente, la fecha y el identificador de la OP de prueba. La tabla distingue los casos positivos (flujo esperado) de los alternativos (validaciones y bloqueos).

**Estados posibles:** `Aprobado`, `Fallido` o `Bloqueado`. Todos los casos se verificaron sobre el ambiente desplegado en el recorrido grabado en video que acompaña esta sección.

**Video Demo:** [DEMO POLYGON](https://youtu.be/-DyVI09W7wE)

| CP | Módulo / tipo | Instrucción y resultado esperado | Estado |
|---|---|---|---|
| S3-01 | Login / positivo | Ingresar con una cuenta de cada rol. Abrir su bandeja, sin acceso a otros módulos. | Aprobado |
| S3-02 | Login / alternativo | Credenciales inválidas o usuario inactivo; se rechaza el ingreso sin crear sesión. | Aprobado |
| S3-03 | Usuarios / permisos | Operario intenta una acción restringida por API; el servidor la deniega. | Aprobado |
| S3-04 | Creación OP / positivo | Crear OP de prueba en el admin de Django con materiales/procesos; comprobar número único y lectura en Producción. | Aprobado |
| S3-05 | Producción / alternativo | Intentar liberar una OP sin datos obligatorios de clasificación/ruta; esperar validación y permanencia en Producción. | Aprobado |
| S3-06 | Picking / positivo | Recibir la OP liberada y enviarla a definición de tandas/Pesaje; consultar la trazabilidad. | Aprobado |
| S3-07 | Supervisor Pesaje / positivo | Definir el número de tandas y verificar la distribución coherente con la cantidad de la OP. | Aprobado |
| S3-08 | Pesaje / alternativo | Registrar valores incompletos, intentar enviar; debe bloquear el avance con mensaje claro. | Aprobado |
| S3-09 | Pesaje y supervisor / positivo | Registrar todas las verificaciones, materiales y tandas; enviar, aprobar o devolver y comprobar el estado. | Aprobado |
| S3-10 | Mezcla / positivo | Supervisor programa, operario registra alistamiento/turnos/parámetros, finaliza y devuelve al circuito de supervisión. | Aprobado |
| S3-11 | Mezcla / alternativo | Dejar un turno abierto o destino inválido e intentar enviar; el backend impide la transición. | Aprobado |
| S3-12 | Ruta especial / positivo | Ejecutar una OP de familia que salta Extrusión; verificar la transición de Mezcla a Calidad. | Aprobado |
| S3-13 | Doble paso / positivo | OP con doble paso especial o total; verificar la secuencia y el número de vuelta/pasada. | Aprobado |
| S3-14 | Extrusión / positivo | Supervisor programa, operario registra datos/turnos/carta y envía conforme a la ruta. | Aprobado |
| S3-15 | Extrusión / alternativo | Condición crítica sin liberar o datos obligatorios ausentes; el avance se rechaza. | Aprobado |
| S3-16 | Hoja azul / positivo | Seguir una OP de acompañamiento IP; completar IP-F-002, liberar y comprobar el historial. | Aprobado |
| S3-17 | Calidad / positivo | Inspeccionar y aprobar una OP; debe pasar a Empaque con el resultado registrado. | Aprobado |
| S3-18 | Calidad / alternativo | Solicitar ajuste parcial/total o reproceso, decidir la ruta como supervisor y comprobar la nueva vuelta. | Aprobado |
| S3-19 | Empaque / positivo | Recibir la OP aprobada, completar el despeje, confirmar el peso de tandas, enviar al supervisor y cerrar con foto. OP `finalizada`. | Aprobado |
| S3-20 | Empaque / alternativo | No cumplir el despeje o superar la tolerancia sin observación; esperar mensaje y bloqueo. | Aprobado |
| S3-21 | Empaque / devolución | El supervisor devuelve la OP con motivo, el operario corrige y reenvía; el historial refleja ambas acciones. | Aprobado |
| S3-22 | PDF e historial / positivo | Tras el cierre en Empaque, descargar el PDF de lote y comprobar datos, `sha256` y acceso según rol. | Aprobado |
| S3-23 | Interfaz / positivo | Identificar hoja normal, urgente y acompañamiento IP; comparar leyenda, lista y detalle. | Aprobado |
| S3-24 | Sesión / alternativo | Cerrar sesión o esperar el vencimiento, reintentar la API con el token viejo; el acceso se rechaza. | Aprobado |
| S3-25 | Salud del servicio | Consultar `GET /api/health/` en el servidor de la planta y comprobar la respuesta `{"status":"ok"}`. | Aprobado |

Casos prioritarios (flujo principal y cierre): S3-01, S3-03, S3-04, S3-06, S3-09, S3-10, S3-14, S3-17, S3-18, S3-19 y S3-22. Un mismo video continuo del recorrido puede servir de soporte para varios casos indicando el minuto de cada uno.

### 6.2 Respaldo de los casos con pruebas automáticas

Muchos de estos casos funcionales están respaldados además por las pruebas automáticas (Sección 2.3), que cubren tanto el flujo esperado como las validaciones. Algunos ejemplos directos:

| Caso funcional | Prueba automática que lo respalda |
|---|---|
| S3-08 (Pesaje bloquea envío incompleto) | `pesaje/tests.py::...test_no_envia_incompleto_ni_crea_borrador_en_error` |
| S3-19 (flujo de Empaque hasta cerrar la OP) | `empaque/tests.py::...test_flujo_completo_hasta_cerrar_la_op` |
| S3-20 (tolerancia de peso exige observación) | `empaque/tests.py::...test_si_la_diferencia_pasa_la_tolerancia_pide_observacion` |
| S3-14/S3-15 (envío a Extrusión y sus validaciones) | `mezcla/tests.py::EnviarExtrusionTests` |

De este modo, la verificación funcional manual (recorrido en la app) y la automática (suite de 241 pruebas) se complementan: la automática protege la lógica en cada cambio y la manual comprueba la experiencia de uso sobre el ambiente desplegado.

### 6.3 Informe de defectos

Los defectos detectados durante las pruebas se registran como issues en GitHub, enlazados a la HU y al PR que los corrige, y se vuelve a ejecutar el caso para confirmar la solución.

---

## 7. Pruebas de usabilidad: protocolo de Sprint 2 y resultados de Sprint 3

### 7.1 Objetivo, población y logística

Las pruebas de usabilidad se ejecutaron **presencialmente en las instalaciones de Sumicolor**, con trabajadores reales de la planta operando Polygon en las estaciones donde usarán el sistema. El objetivo fue verificar que cada operario pudiera **ubicar**, **entender**, **registrar** y **transferir** una Orden de Producción con la menor asistencia posible, de acuerdo con el protocolo planeado en el Sprint 2 (`docs/usabilidad/protocolo.md`) y con los requisitos no funcionales de usabilidad del Sprint 0 (RNF-06 interfaz minimalista, RNF-07 accesibilidad, RNF-08 compatibilidad).

**Participantes.** Se trabajó con un participante por cada proceso del flujo, para cubrir el recorrido completo de la OP. Los nombres se omiten por privacidad y se identifican por rol:

| Participante | Proceso / rol | Perfil |
|---|---|---|
| P01 | Producción / Planeación | Operario que hoy inicia la OP en papel |
| P02 | Picking | Operario de alistamiento de materiales |
| P03 | Pesaje | Operario de pesaje de materias primas |
| P04 | Mezcla | Operario de mezcla |
| P05 | Extrusión | Operario de extrusión |
| P06 | Calidad | Inspector de calidad |
| P07 | Empaque | Operario de empaque |
| P08 | Supervisor | Supervisor de proceso (Pesaje/Mezcla/Extrusión/Empaque) |

**Logística.** Sesiones individuales moderadas, en la estación de trabajo de cada participante, con un moderador que guía y un observador que toma notas. Cada participante ejecutó la tarea propia de su rol sobre una OP de prueba con datos ficticios. Al finalizar se aplicó un cuestionario breve de satisfacción.

- **Lugar:** instalaciones de Sumicolor S. A.
- **Fecha:** miércoles, 4 de noviembre.
- **Evidencia:** fotografías tomadas durante las sesiones

Evidencia 1

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/b12bc45d-e791-4190-94d2-74ed7e146df7" />

Evidencia 2

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/4dc8367e-5046-49ec-ad01-361ccd91a36f" />

Evidencia 3

<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/8371234b-98c0-477f-b088-09af411afa3c" />

### 7.2 Guion del moderador

Cada sesión siguió el mismo guion, para que todos los participantes vivieran la misma experiencia:

1. **Bienvenida y contexto (2 min).** «Gracias por participar. Vamos a probar la aplicación Polygon, no lo vamos a evaluar a usted. Queremos ver qué tan fácil o difícil le resulta usarla para mejorarla. No hay respuestas correctas ni incorrectas.»
2. **Consentimiento (1 min).** Se explica que se tomarán notas y fotografías solo con fines del proyecto, sin publicar datos personales.
3. **Pregunta inicial (1 min).** «¿Cómo hace hoy este registro en papel? ¿Qué es lo que más tiempo le toma?»
4. **Ejecución de la tarea (5–8 min).** Se entrega la tarea del rol (Sección 7.3) y se le pide que piense en voz alta. El moderador no ayuda salvo bloqueo total.
5. **Preguntas de seguimiento (3 min).** Las de la Sección 7.5.
6. **Satisfacción (1 min).** Calificación de 1 a 5 sobre la facilidad de uso.
7. **Cierre y agradecimiento.**

### 7.3 Tareas por rol

Las tareas derivan del protocolo del Sprint 2 y de las responsabilidades de cada rol descritas en el Sprint 0:

| Tarea | Rol | Qué debía hacer | Criterio de éxito |
|---|---|---|---|
| U-01 | Varios roles | Diferenciar una OP normal, urgente y de acompañamiento IP e indicar el significado de los colores | Identifica los tres tipos sin ayuda |
| U-02 | Mezcla | Encontrar su OP, registrar y guardar correctamente un turno | Guarda el turno y la OP avanza |
| U-03 | Varios roles | Identificar la etapa actual de la OP y la siguiente acción | Reconoce el estado y qué sigue |
| U-04 | Pesaje | Completar verificaciones y materiales y enviar a Supervisión | Envía con los datos completos |
| U-05 | Operario | Enviar un formulario incompleto, leer el error y corregirlo | Entiende el mensaje y corrige |
| U-06 | Calidad / Empaque | Registrar una inspección y, en Empaque, cerrar una OP | Completa su etapa correctamente |
| U-07 | Supervisor | Revisar una OP y aprobarla o devolverla con motivo | Toma la decisión y la OP cambia de estado |

### 7.4 Instrumentos de registro

Durante cada sesión se usaron tres instrumentos sencillos:

- **Hoja de observación:** para cada tarea, el observador anota si el participante la completó sin ayuda, si necesitó una indicación y cualquier bloqueo o duda visible.
- **Cuestionario de satisfacción:** una pregunta de cierre, «Del 1 al 5, ¿qué tan fácil le resultó usar la aplicación para su tarea?».
- **Notas cualitativas:** comentarios espontáneos del participante al pensar en voz alta.

El registro se centró en lo observable (éxito con o sin ayuda, puntos de duda, comentarios) más que en una cronometría exhaustiva, dado el carácter práctico de las sesiones en planta.

### 7.5 Resultados y hallazgos

El recorrido completo de la OP se pudo ejecutar con los participantes de cada proceso. En términos generales, los operarios lograron completar la tarea de su rol, y la reacción al reemplazo del papel por la aplicación fue favorable. Los hallazgos cualitativos se resumen así:

| # | Observación de los participantes | Proceso | Tipo | Acción tomada |
|---|---|---|---|---|
| H1 | La leyenda de colores de las OP (normal/urgente/hoja azul) se entendió de inmediato y ayudó a priorizar | Varios roles | Positivo | Se conserva |
| H2 | Los mensajes de validación que impiden avanzar con datos incompletos se consideraron claros y útiles para no equivocarse | Pesaje / Mezcla | Positivo | Se conserva |
| H3 | El historial de la OP y la visibilidad del estado actual facilitaron saber «en qué va» la orden | Varios roles | Positivo | Se conserva |
| H4 | El despeje de línea y el registro de pesos por tanda en Empaque resultaron comprensibles | Empaque | Positivo | Se conserva |
| H5 | Algunos operarios con menos experiencia tecnológica pidieron una indicación la primera vez para ubicar el botón de envío a la siguiente etapa | Varios roles | Mejora menor | Reforzar en el manual (Sección 8) e inducción |
| H6 | Se sugirió que los botones de acción principal fueran aún más visibles en pantallas pequeñas (tabletas) | Varios roles | Mejora menor | Registrada para una iteración futura |

**Comparación con las hipótesis del Sprint 2.** Las hipótesis de diseño (interfaz minimalista, mensajes de error claros, orientación dentro del flujo) se vieron respaldadas por la reacción de los participantes: la leyenda de colores (H1), las validaciones (H2) y la visibilidad del estado (H3) funcionaron como se esperaba. Las fricciones detectadas (H5, H6) son menores y de interfaz, no de lógica del proceso.

**Acciones derivadas.** Las mejoras menores no requieren cambios estructurales: H5 se atiende con el manual de usuario y la inducción inicial; H6 queda registrada como ajuste de interfaz para una iteración futura. No se identificaron bloqueos que impidieran completar el flujo ni que exijan reabrir una historia de usuario en un hipotético Sprint 4.

**Conclusión de usabilidad.** Los usuarios reales de Sumicolor pudieron operar Polygon en sus estaciones y valoraron positivamente la digitalización del proceso. Las observaciones de mejora son de interfaz y acompañamiento, no de funcionalidad, lo que respalda la viabilidad operativa del sistema (Sección 4.6).

### 7.6 Resultados por participante

Resumen de la observación de cada participante durante su tarea. La valoración es cualitativa (según lo observado en planta por el moderador), al no haberse realizado una cronometría formal en estas sesiones de trabajo:

| Participante | Proceso / rol | Tarea | ¿Completó la tarea? | ¿Necesitó ayuda? | Satisfacción percibida |
|---|---|---|---|---|---|
| P01 | Producción / Planeación | U-03 | Sí | No | Alta |
| P02 | Picking | U-03 | Sí | No | Alta |
| P03 | Pesaje | U-04 | Sí | No | Alta |
| P04 | Mezcla | U-02 | Sí | Una indicación inicial | Alta |
| P05 | Extrusión | U-03 | Sí | No | Media-alta |
| P06 | Calidad | U-06 | Sí | No | Alta |
| P07 | Empaque | U-06 | Sí | No | Alta |
| P08 | Supervisor | U-07 | Sí | No | Alta |

**Lectura de los resultados.** Todos los participantes completaron la tarea de su rol. La única asistencia registrada fue una indicación inicial para ubicar el botón de envío (hallazgo H5), coherente con una mejora de interfaz menor. La satisfacción percibida fue alta de forma generalizada, lo que respalda que la aplicación resulta usable para el personal de planta.

---

## 8. Manual de usuario — Polygon

> **Destinatarios:** operarios, supervisores y administrador de Sumicolor. El manual guía paso a paso cada rol del proceso. Se adjunta un video demo con el uso de la plataforma en su totalidad.

### 8.1 Requisitos y acceso

1. Usar un navegador actualizado (Chrome o Edge) desde un computador o una tableta de la planta conectados a la red de Sumicolor.
2. Abrir **`http://10.1.1.199:8080`** (dirección de Polygon en el servidor de la planta).
3. Escribir el usuario y la contraseña asignados por la administración.
4. Pulsar **Ingresar**. Polygon lleva a la pantalla inicial según el rol. Si las credenciales no son válidas, aparece un mensaje de error; solicitar apoyo a la administración en lugar de usar otra cuenta.
5. Al terminar el turno, usar la opción de **cerrar sesión** del menú. La sesión también vence automáticamente a las 12 horas. No compartir credenciales ni dejar el acceso abierto en estaciones comunes.

### 8.2 Administrador: cuentas y consulta general

**Ruta web:** `/admin/usuarios`.

1. Ingresar con cuenta administradora y acceder a **Usuarios**.
2. Crear una cuenta, indicar credenciales y seleccionar uno de los roles disponibles para la estación/función.
3. Guardar, buscar al usuario en el listado y comprobar su rol/estado.
4. Para editar o desactivar, abrir el registro correspondiente, cambiar solo los campos autorizados y guardar.
5. Para la carga inicial de una OP (que proviene de Visual), usar el **panel de administración** de Polygon con la cuenta de administrador. La carga incluye el encabezado, los materiales y los procesos de la orden. No hay un botón de «crear OP» en la lista de Producción: la OP llega ya definida.
6. Para revisar las órdenes finalizadas, consultar el historial de órdenes; los resúmenes de lote (PDF) y los tiempos de novedades están disponibles desde las secciones correspondientes.

### 8.3 Producción: revisar y liberar OP

**Ruta web:** `/admin/produccion/ordenes`.

1. Acceder a la bandeja y seleccionar una OP cargada previamente.
2. Verificar número, referencia, cantidad, materiales y datos de la hoja de procesos.
3. Completar o corregir en el detalle la clasificación, stickers, grupo de Pesaje y ruta/destino permitido, según controles disponibles.
4. Identificar el tipo de hoja: **normal/blanca**, **urgente/amarilla** o **acompañamiento IP/azul**; consultar su leyenda en la lista.
5. Ejecutar **enviar a Picking** cuando estén completos los datos. Si aparece una validación, revisar el campo señalado y volver a intentar.
6. Consultar el historial de la OP para confirmar el cambio de estado.

### 8.4 Picking: preparar y entregar

**Ruta web:** `/admin/picking/ordenes`.

1. Abrir la OP liberada por Producción y verificar referencia y materiales.
2. Registrar la recepción de Picking según la acción disponible.
3. Confirmar el alistamiento y, cuando esté completo, enviar a Pesaje.
4. Para consultar órdenes ya entregadas, usar el filtro de órdenes liberadas de la bandeja.

### 8.5 Supervisor y Pesaje: definición, registro y revisión

**Rutas web:** `/admin/supervision-pesaje/ordenes` (supervisor) y `/admin/pesaje/ordenes` (operario).

1. **Supervisor antes del pesaje:** abrir la OP pendiente de definición de tandas, elegir la cantidad aplicable y enviarla al operario. Verificar que la suma de las tandas cubra la cantidad de la OP.
2. **Operario Pesaje:** abrir la OP, iniciar el proceso cuando corresponda, diligenciar verificaciones de seguridad y condiciones (incluidas críticas si aplica).
3. Registrar el avance de los materiales y las cantidades de las tandas; guardar los datos.
4. Revisar advertencias de campos obligatorios y no intentar omitir una verificación crítica. Cuando esté completo, usar **enviar a supervisor**.
5. **Supervisor:** abrir la bandeja de revisión, comparar cantidades/datos y elegir **aprobar** o **devolver**. Si devuelve, justificar el cambio donde se solicite y pedir nueva presentación del registro.
6. Consultar el nuevo estado y la trazabilidad para confirmar que el proceso avanzó o volvió a Pesaje.

### 8.6 Supervisor y operario de Mezcla

**Rutas web:** `/admin/supervision-mezcla/ordenes`, `/admin/mezcla/ordenes`, `/admin/mezcla/registro/:id`.

1. El supervisor recibe la OP y registra/programa la etapa según el tipo de mezcla, destino y recursos del proceso.
2. El operario abre la OP autorizada; completa alistamiento y consulta **hoja de proceso** con las indicaciones vigentes de la referencia, si existen datos cargados.
3. Inicia y registra las tandas/turnos y los parámetros reales exigidos; guarda el formulario.
4. Finaliza el turno. Si permanece abierto o falta información, el servidor impedirá avanzar.
5. Solicita el envío a supervisión según la vista. El supervisor revisa y autoriza el destino siguiente definido por la OP (Extrusión o directamente Calidad según ruta).
6. Si la OP contiene doble paso, seguir la vuelta/pasada que indique Polygon; no terminarla manualmente al concluir la primera etapa.

### 8.7 Supervisor y operario de Extrusión

**Rutas web:** `/admin/supervision-extrusion/ordenes`, `/admin/extrusion/ordenes`, `/admin/extrusion/registro/:id`.

1. El supervisor programa los datos de la etapa y libera la OP al operario.
2. El operario revisa las instrucciones/carta de procesos disponibles y completa el despeje o condiciones del equipo.
3. Registra turno, parámetros reales y verificaciones de pellet; guarda los cambios.
4. Completa los requisitos para enviar a supervisor, o para el paso autorizado siguiente en la ruta.
5. En doble paso total, comprobar si se inicia una nueva pasada de Extrusión; en el resto, comprobar el avance a Calidad cuando proceda.
6. Ante devolución o validación, corregir y repetir el registro sin crear otra OP.

### 8.8 Ingeniería de Producción: hoja azul

**Ruta web:** `/admin/ip/seguimiento`.

1. Ingresar con rol **Ing. Producción**.
2. Consultar únicamente OP clasificadas para acompañamiento IP.
3. Seleccionar la orden y abrir el seguimiento del formato IP-F-002.
4. Completar los datos exigidos por la etapa, revisar el prellenado y guardar la información.
5. Liberar el seguimiento o devolverlo con la observación correspondiente y revisar el estado resultante.

### 8.9 Calidad y supervisor de novedades

**Rutas web:** `/admin/calidad/ordenes`, `/admin/novedades-calidad/ordenes`.

1. El inspector de Calidad abre la OP, registra recepción, inspección y resultados solicitados.
2. Si cumple, utiliza **aprobar** para remitir la OP a Empaque.
3. Si no cumple, selecciona **ajuste** o **reproceso**, registra los campos y motivos requeridos y comprueba que la OP cambie a **novedad de Calidad**.
4. El **supervisor de novedades** consulta la orden, define el destino o la ruta de reproceso y la remite a la estación correspondiente.
5. Cuando la OP regrese, Calidad realiza la evaluación que corresponda a la nueva vuelta; evitar registrar una aprobación sin la inspección necesaria.

### 8.10 Empaque, revisión final y cierre de la orden

**Rutas web:** `/admin/empaque/ordenes` (operario) y `/admin/supervision-empaque/ordenes` (supervisor).

1. El operario recibe la OP aprobada por Calidad, abre el detalle y registra su nombre/recepción.
2. Completa las verificaciones del **despeje de línea**. Todos los ítems exigidos deben cumplir antes de registrar los pesos.
3. Confirma el **peso real de cada tanda** contra el esperado; si la diferencia supera la tolerancia definida por la aplicación, registra la observación solicitada.
4. Cuando esté completo, utiliza **Enviar a supervisor**.
5. El supervisor revisa la información: si encuentra una novedad, selecciona **Devolver al operario** y escribe el motivo. Si es correcta, selecciona **Cerrar OP** y aporta la **fotografía de autorización** requerida.
6. Una OP cerrada debe quedar en estado **Finalizada**. El cierre invoca la generación del **PDF de resumen de lote**; comprobar su aparición en el historial/resúmenes, ya que un error de generación puede dejar la OP finalizada sin PDF.
7. El administrador consulta el histórico y el resumen desde las secciones correspondientes; Calidad dispone de una lista de resúmenes según sus permisos.

### 8.11 Consulta, mensajes y soporte

| Situación | Qué debe hacer el usuario |
|---|---|
| No inicia sesión | Revisar credenciales; si persiste, pedir al administrador revisión de cuenta/rol |
| Una OP no aparece | Verificar que esté en la etapa asignada al rol y comprobar filtros; consultar con supervisor |
| No deja enviar | Leer el mensaje, terminar campos/turnos/verificaciones y revisar autorización del supervisor |
| Aparece `403`/permiso | No utilizar otra cuenta; solicitar revisión de permisos |
| Se cerró la sesión | Volver a ingresar; los tokens vencen y pueden invalidarse al cambiar contraseña |
| Parece no actualizarse una bandeja | Recargar o esperar actualización periódica; consultar última actualización y conexión |
| La OP figura finalizada pero no se ve PDF | Informar el identificador a administración/soporte, sin volver a cerrar la OP |
| Error o pérdida de conexión | Registrar hora, OP, pantalla y mensaje; verificar antes de intentar duplicar acciones |

**Canal de soporte:** Vbenjumea@sumicolor.com.co Nombre: Edisson Vladimir Benjumea González

**Video Demo:** [DEMO POLYGON](https://youtu.be/-DyVI09W7wE)

---

## 9. Despliegue de la aplicación

### 9.1 Modalidad de despliegue: on-premise en Sumicolor

Polygon está **desplegado y en operación en el servidor propio de Sumicolor**, dentro de la red de la planta. Los operarios acceden desde sus computadores y tabletas a la dirección **`http://10.1.1.199:8080`**.

**Por qué on-premise y no en una nube pública.** El documento maestro plantea el despliegue "en una nube pública", pero Polygon es un caso particular: Sumicolor ya cuenta con **infraestructura de servidores propia** y pidió expresamente que el sistema corriera **dentro de su red**, no en un proveedor externo como AWS, Azure o GCP. Las razones son legítimas y de negocio:

- Los datos de producción de la empresa permanecen dentro de su propia infraestructura, sin salir a un tercero.
- La aplicación convive con los sistemas que Sumicolor ya opera en ese servidor.
- No se incurre en costos recurrentes de nube, aprovechando una inversión de infraestructura ya existente (ver Sección 4.4).

Por eso la aplicación no se publica en una URL abierta a Internet: su "nube" es la red interna de la planta, que es donde los usuarios finales realmente la necesitan. Esta decisión de arquitectura se acordó con el cliente.

### 9.2 Arquitectura del despliegue

El servidor es un Windows Server que atiende la aplicación en el puerto 8080. Una petición de un operario sigue este camino:

```text
Navegador del operario (red interna de Sumicolor)
        │ http://10.1.1.199:8080
        ▼
IIS (servidor web de Windows)
   ├── /          → aplicación Vue compilada
   ├── /static/   → archivos estáticos del panel de administración
   └── /api/      → reenvío a la aplicación Django
                       ▼
               Waitress (servidor de aplicación, como servicio de Windows)
                       ▼
               Django / DRF
                       ├── PostgreSQL (base de datos de Polygon)
                       └── Carpeta de medios (fotos de autorización)
```

| Componente | Función |
|---|---|
| IIS | Servidor web: entrega la aplicación y reenvía las llamadas de `/api` a Django |
| Waitress | Ejecuta Django como un servicio de Windows, de modo que arranca solo al reiniciar el servidor |
| Django / DRF | Lógica de negocio, validaciones, estados de la OP y API REST |
| PostgreSQL | Base de datos propia de Polygon (independiente de Visual) |
| Carpeta de medios | Almacena las fotos de autorización que van en el PDF del lote |

> La base de datos (PostgreSQL) y el servidor de aplicación (Waitress) solo escuchan dentro del propio servidor; desde la red únicamente se expone el puerto 8080 de IIS. Esto reduce la superficie de exposición del sistema.

### 9.3 Proceso de despliegue

El despliegue fue **manual y documentado**. El procedimiento completo, con los comandos y la configuración del servidor, se conserva en un documento técnico interno del equipo que **no se publica** porque contiene la IP, la estructura de carpetas y referencias a credenciales del servidor de Sumicolor. A alto nivel, los pasos fueron:

1. Instalar en el servidor las herramientas base: Python, Node.js (solo para compilar el frontend), PostgreSQL, el servidor de aplicación y los módulos de IIS para reenviar las llamadas a Django.
2. Crear la base de datos de Polygon y definir la configuración del entorno (credenciales y parámetros) **fuera del repositorio**, solo en el servidor.
3. Instalar las dependencias del backend, aplicar las migraciones, recopilar los archivos estáticos y crear el usuario administrador.
4. Compilar el frontend y publicarlo en IIS, configurando el reenvío de `/api` hacia Django.
5. Registrar la aplicación Django como servicio de Windows para que arranque automáticamente.
6. Abrir el puerto 8080 en el firewall del servidor para la red interna y ajustar los permisos de lectura de IIS.
7. Cargar los catálogos y las cartas de proceso de Sumicolor, y crear las cuentas de cada rol.
8. Verificar el funcionamiento con un recorrido completo de una OP de prueba desde otro equipo de la red.

### 9.4 Comprobación de funcionamiento

La aplicación responde correctamente en la red de la planta. Una comprobación rápida del servicio es el endpoint de salud:

```text
GET http://10.1.1.199:8080/api/health/   →   {"status": "ok"}
```

Más allá del chequeo de salud, el funcionamiento se validó con el recorrido completo de una OP (login por rol, avance entre estaciones, cierre en Empaque y descarga del PDF de lote), que corresponde a los casos de prueba de la Sección 6.

### 9.5 Mantenimiento, respaldo y seguridad

| Aspecto | Estado |
|---|---|
| Actualización de versión | Procedimiento manual documentado (reemplazar código, migrar, recompilar y reiniciar el servicio) |
| Respaldo | Copia de la base de datos PostgreSQL y de la carpeta de fotos de autorización |
| Secretos | Fuera del repositorio; solo en la configuración del servidor |
| Exposición de red | Solo el puerto 8080 (IIS) visible en la red; base de datos y aplicación no expuestas |
| HTTPS | El acceso es por HTTP en la red interna; queda habilitar HTTPS si TI entrega un certificado |

<img width="1600" height="812" alt="image" src="https://github.com/user-attachments/assets/1cffb279-1d3b-4429-89db-b2f3c1e0725b" />

---

## 10. Evidencias de ceremonias Scrum — Sprint 3

El documento maestro pide conservar, por cada sprint, las evidencias de las ceremonias y reuniones del proyecto. El Sprint 3 se trabajó de forma estrecha con el cliente: las reuniones clave se realizaron **con el Product Owner de Sumicolor y su equipo** (gerencia y jefatura de ingeniería de procesos), más las sesiones de trabajo en la planta. Las reuniones realizadas fueron:

| Reunión | Participantes | Objetivo y acuerdos | Evidencia |
|---|---|---|---|
| Reunión de seguimiento con el cliente | PO de Sumicolor, gerente, jefe de ingeniería de procesos y equipo de desarrollo | Presentación del avance del Sprint y alineación de prioridades con el cliente | [Reunión de seguimiento con el cliente](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Ceremonias-Sprint-3#acta-de-reuni%C3%B3n--seguimiento-con-el-cliente-sprint-3) |
| Sprint Review con el cliente | PO de Sumicolor, gerente, jefe de ingeniería de procesos y equipo de desarrollo | Demostración de las funcionalidades del Sprint 3 y aceptación del Product Owner | [Sprint Review con el cliente](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Ceremonias-Sprint-3#acta-de-reuni%C3%B3n--sprint-review-con-el-cliente-sprint-3) |
| Sesión de pruebas de usabilidad | Operarios de Sumicolor por proceso y equipo de desarrollo | Ejecución del protocolo de usabilidad en planta (ver Sección 7) | [Sesión de pruebas de usabilidad](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Ceremonias-Sprint-3#acta-de-reuni%C3%B3n--sesi%C3%B3n-de-pruebas-de-usabilidad-sprint-3) |
| Sesión de despliegue en Sumicolor | Equipo de desarrollo y TI de Sumicolor | Instalación y puesta en marcha de Polygon en el servidor de la planta (ver Sección 9) | [Sesión de despliegue en Sumicolor](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Ceremonias-Sprint-3#acta-de-reuni%C3%B3n--sesi%C3%B3n-de-despliegue-en-sumicolor-sprint-3) |

> El trabajo del equipo de desarrollo se coordinó directamente en el tablero de GitHub Projects (issues, ramas y Pull Requests) y en el flujo de trabajo diario, más que en ceremonias internas formales. Las reuniones formales del Sprint 3 se concentraron en la relación con el cliente, que es donde se tomaron las decisiones de alcance y la aceptación de la entrega.

**Fecha reunión de seguimiento con el cliente:** viernes, 16 de octubre

**Fecha del sprint review con el cliente:** miércoles, 21 de octubre

**Fecha de la sesión de pruebas de usabilidad:** miércoles, 4 de noviembre

**Fecha de la sesión de despliegue en SUMICOLOR:** viernes, 6 de noviembre

---

## 11. Sustentación del Sprint 3

### 11.1 Presentación al cliente

La Sprint Review con Sumicolor se orientó al proceso real de la planta y siguió este hilo: el problema inicial (registro en papel), la solución y su alcance, la demostración de una OP normal hasta su cierre en Empaque, una variante de Calidad con ajuste/reproceso, el rol de supervisor y la hoja azul, las pruebas realizadas y el cierre con las funcionalidades aceptadas. Se expuso con claridad lo entregado frente a lo que queda como evolución futura (integración con Visual y reportes ampliados).

### 11.2 Presentación al profesor

Guion de la demostración frente a los profesores, con la correspondencia a cada área que evalúa el documento maestro:

| Área evaluada | Qué mostrar en la demo | Soporte |
|---|---|---|
| Desarrollo de código | Un flujo completo por roles hasta `finalizada` y una ruta excepcional; mostrar backend, frontend y Pull Requests | OP de prueba, commit final y video del recorrido |
| Despliegue | La aplicación funcionando en el servidor de Sumicolor (on-premise), desde la red de la planta | Sección 9 y capturas del despliegue |
| Caso de negocio | Presupuesto preoperativo y de operación, matriz de riesgos y conclusión de viabilidad | Sección 4 |
| Pruebas de usabilidad | Perfil de participantes, tareas, hallazgos y mejoras | Sección 7 y fotos de las sesiones |
| Manual de usuario | Guiar una acción de dos o más roles con las capturas del manual | Sección 8 (M-01 a M-20) |
| Calidad / automatización | Resultado de las pruebas (241 OK), cobertura 94 %, CI en GitHub Actions | Sección 2 y Sección 6 |


---

## 12. Evidencias de soporte

Relación de las evidencias gráficas y documentales que acompañan cada entregable. El contenido escrito está en las secciones anteriores; aquí se consolidan los soportes visuales que se adjuntan a la Wiki.

| ID | Entregable | Soporte a adjuntar | Sección |
|---|---|---|---|
| E01 | Producto | Video del recorrido de una OP (normal, variante especial y ajuste/reproceso) hasta el cierre con PDF | 1 y 6 |
| E02 | Backlog | Captura del tablero de GitHub Projects filtrado por Sprint 3 con las 7 HU en Done | 3 |
| E03 | Ceremonias | Actas de las reuniones con el cliente y fotos de las sesiones en planta | 10 |
| E04 | Caso de negocio | Visto bueno del Product Owner sobre los supuestos del caso de negocio | 4 |
| E05 | Pruebas funcionales | Video del recorrido como bitácora de los casos S3-01 a S3-25 | 6 |
| E06 | Pruebas automáticas / CI | Captura de la corrida del CI en verde y de `coverage report` (94 %) | 2 |
| E07 | Usabilidad | Fotos de las sesiones de usabilidad en planta | 7 |
| E08 | Manual de usuario | Capturas M-01 a M-20 de la aplicación desplegada | 8 |
| E09 | Despliegue | Captura de la app en el servidor de Sumicolor y la confirmación del cliente | 9 |
| E10 | Sustentación | Archivo de la presentación usada en la sustentación | 11 |

**Prioridad según la rúbrica:** usabilidad y despliegue pesan 20 puntos cada uno, igual que el desarrollo final; el caso de negocio y el manual, 15 cada uno; la presentación, 10. Las evidencias E06, E07, E08 y E09 son las de mayor impacto en la nota.

---

## 13. Correcciones y coherencia con sprints anteriores

Al consolidar el producto final se ajustaron algunas descripciones de sprints anteriores para que coincidan con lo realmente entregado:

1. **Tecnología de persistencia.** La base de datos de Polygon es **PostgreSQL**. SQL Server aparece únicamente como el origen de Visual en una integración futura, no como la base del sistema.
2. **Configurabilidad (HU-07/HU-08).** El alcance final describe los módulos del flujo entregado. El motor de configuración dinámica de módulos y formularios por el cliente, planteado en el Sprint 0, quedó fuera del alcance del MVP.
3. **Creación de la OP.** La OP se carga por el panel de administración (proviene de Visual), no por un formulario en la lista de Producción. Producción revisa y libera la OP.
4. **Denominación de Picking.** Se usa **Picking** en roles y textos. La carpeta `apps/picky` es heredada y no está instalada.
5. **Reportes.** La app `apps/reportes` es estructural; los reportes del Sprint 3 (resumen de lote en PDF y consulta de tiempos) pertenecen a `calidad` y `empaque`.
6. **Momento de generación del PDF de lote.** El PDF se genera al **cerrar la OP en Empaque** (`CerrarOrdenView`), no al aprobar en Calidad.
7. **Alcance de Empaque.** El Sprint 3 entrega el módulo completo de Empaque (despeje de línea, pesos por tanda, revisión del supervisor y cierre), que en el Sprint 2 figuraba como pendiente.
8. **Calidad y CI.** El backend tiene 241 pruebas que pasan (`OK`) con 94 % de cobertura, y el CI exige un umbral de cobertura del 90 %.
9. **Despliegue.** El sistema está desplegado on-premise en el servidor de Sumicolor (ver Sección 9); se documenta esa modalidad y no un hospedaje en nube pública externa.

---

## 14. Fuentes

Esta página se elaboró a partir de las siguientes fuentes del propio proyecto:

- **Código y configuración** del repositorio: [README](https://github.com/SimonCastroV/Polygon/blob/main/README.md), [CI](https://github.com/SimonCastroV/Polygon/blob/main/.github/workflows/ci.yml), [settings](https://github.com/SimonCastroV/Polygon/blob/main/Backend/config/settings.py), [modelo de la OP](https://github.com/SimonCastroV/Polygon/blob/main/Backend/apps/produccion/models.py), [rutas del frontend](https://github.com/SimonCastroV/Polygon/blob/main/Frontend/src/router/index.js), [permisos de consulta de OP](https://github.com/SimonCastroV/Polygon/blob/main/Backend/apps/produccion/views.py), [Empaque](https://github.com/SimonCastroV/Polygon/blob/main/Backend/apps/empaque/views.py) y [resumen de lote](https://github.com/SimonCastroV/Polygon/blob/main/Backend/apps/calidad/resumen_lote.py).
- **Tablero de GitHub Projects** del equipo, con las HU, issues y Pull Requests del Sprint 3.
- **Historias de usuario** en `docs/context/HistoriasUsuarioPolygon/HUs_Polygon.md` y **protocolo de usabilidad** en `docs/usabilidad/protocolo.md`.
- **Wikis de sprints anteriores:** [Sprint 0](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Sprint%E2%80%900%E2%80%90Proyecto%E2%80%902), [Sprint 1](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Sprint%E2%80%901%E2%80%90Proyecto%E2%80%902) y [Sprint 2](https://github.com/Svillamizar05/Polygon-Wiki/wiki/Sprint%E2%80%902%E2%80%90Proyecto%E2%80%902).
- **Documento maestro** del Sprint 3 (rúbrica de entregables y pesos).

Los resultados de pruebas (241 OK, 94 % de cobertura) provienen de la ejecución real de la suite. Los valores del caso de negocio (Sección 4) son estimaciones de mercado con fines del caso de negocio, no cotizaciones contratadas.
