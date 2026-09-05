# Sprint 1 - Proyecto 2 - MVP Versión 1

## 1. Información general

**Proyecto:** Polygon  
**Cliente:** Sumicolor  
**Sprint:** Sprint 1  
**Versión:** MVP V1  

Polygon es un sistema orientado a la gestión y trazabilidad de las Órdenes de Producción de Sumicolor. El sistema busca digitalizar la información generada durante el proceso productivo y permitir que las diferentes áreas involucradas consulten y registren la información correspondiente a cada etapa.

El proceso contemplado por Polygon incluye etapas como:

- Planeación
- Picky
- Pesaje
- Mezcla
- Extrusión
- Control de Calidad
- Empaque
- Seguimiento y reportes

---

# 2. Objetivo del Sprint

El objetivo del Sprint 1 es construir la primera versión funcional de Polygon, estableciendo la base técnica del sistema y desarrollando las funcionalidades iniciales necesarias para gestionar usuarios y comenzar el ciclo de vida de una Orden de Producción.

El Sprint busca obtener un MVP que permita demostrar un primer flujo funcional desde el acceso al sistema hasta la creación, consulta y administración inicial de una Orden de Producción.

## Sprint Goal

> Contar con una primera versión funcional de Polygon que permita controlar el acceso de los usuarios y gestionar la etapa inicial de las Órdenes de Producción, dejando preparada la arquitectura necesaria para continuar con las etapas operativas del proceso productivo en los siguientes sprints.

---

# 3. Alcance del Sprint 1

Debido a que este corresponde al primero de tres sprints de desarrollo, se priorizan las funcionalidades que sirven como base para el resto del sistema.

## Historias de Usuario propuestas para el Sprint 1

### Autenticación y administración

- HU-01 - Iniciar sesión (#9)
- HU-02 - Cerrar sesión (#10)
- HU-04 - Crear usuario (#12)
- HU-05 - Editar usuario (#13)
- HU-06 - Asignar roles y permisos (#14)
- HU-09 - Gestionar catálogos (#17)

### Planeación de Órdenes de Producción

- HU-10 - Crear Orden de Producción (#18)
- HU-11 - Editar Orden de Producción (#19)
- HU-12 - Consultar Órdenes de Producción (#20)
- HU-13 - Liberar Orden de Producción a Picky (#21)
- HU-14 - Cancelar Orden de Producción (#22)

Las siguientes historias pueden ser incorporadas al Sprint 1 si la capacidad del equipo lo permite o trasladarse a un sprint posterior:

- HU-03 - Recuperar contraseña (#11)
- HU-07 - Configurar módulos de producción (#15)
- HU-08 - Configurar formularios (#16)

La priorización busca obtener primero un flujo funcional completo antes de incorporar funcionalidades administrativas de mayor complejidad.

---

# 4. Desarrollo del MVP V1

## Arquitectura tecnológica

Polygon se desarrolla utilizando una arquitectura separada entre backend y frontend.

### Backend

- Python
- Django
- Django REST Framework
- PostgreSQL
- django-cors-headers
- python-decouple

El backend se encuentra organizado mediante aplicaciones independientes que representan diferentes dominios del proceso productivo:

- usuarios
- produccion
- picky
- pesaje
- mezcla
- extrusion
- calidad
- empaque
- reportes

### Frontend

- Vue 3
- Vite
- Vue Router
- Pinia
- Axios

El frontend será responsable de proporcionar las interfaces necesarias para que cada tipo de usuario interactúe con los módulos permitidos según su rol.

## Estado inicial del Sprint

Al inicio del Sprint 1 se creó la estructura base del proyecto, incluyendo:

- Proyecto Django.
- Aplicaciones correspondientes a los principales módulos del sistema.
- Conexión preparada para PostgreSQL.
- Django REST Framework.
- Configuración de CORS.
- Proyecto frontend en Vue 3.
- Configuración inicial de Vite.
- Instalación de Axios, Pinia y Vue Router.
- Separación entre Backend y Frontend.

A partir de esta estructura se desarrollarán las funcionalidades comprometidas para el MVP V1.

---

# 5. Primera versión del Caso de Negocio

# 5.1 Descripción del Negocio

Sumicolor cuenta con un proceso productivo compuesto por diferentes etapas que deben ejecutarse de manera secuencial para completar una Orden de Producción.

Durante estas etapas se genera información relacionada con materiales, cantidades, procesos realizados, responsables, controles de calidad y estado de cada Orden de Producción.

Actualmente parte de esta información depende de formatos y registros que dificultan mantener una trazabilidad centralizada del proceso.

Polygon surge como una solución para centralizar digitalmente la información de las Órdenes de Producción y permitir que cada área registre la información correspondiente a su etapa.

Esto permite que una Orden de Producción pueda ser seguida desde Planeación hasta su finalización.

## Misión

Digitalizar y centralizar la gestión de las Órdenes de Producción de Sumicolor mediante una plataforma que permita registrar, consultar y mantener la trazabilidad de la información generada durante el proceso productivo.

## Visión

Consolidar Polygon como la plataforma central para la gestión del proceso productivo de Sumicolor, facilitando el seguimiento de las Órdenes de Producción, reduciendo la dependencia de registros físicos y mejorando la disponibilidad y trazabilidad de la información.

---

# 5.2 Análisis del Mercado

## Investigación de mercado

Polygon se encuentra orientado inicialmente a resolver una necesidad específica dentro del proceso productivo de Sumicolor: disponer de una herramienta centralizada que permita administrar las Órdenes de Producción y conservar la trazabilidad entre las diferentes etapas de fabricación.

La necesidad identificada se relaciona con procesos donde diferentes áreas deben intercambiar información de manera secuencial y donde conocer el estado actual e historial de una Orden de Producción resulta fundamental para el control del proceso.

El sistema pertenece al contexto de soluciones empresariales destinadas a la digitalización y gestión de procesos productivos.

### Pendiente para complementar esta sección

Para completar la investigación de mercado se incorporarán fuentes externas y actualizadas relacionadas con:

- Digitalización de procesos industriales.
- Sistemas de gestión de producción.
- Trazabilidad en procesos manufactureros.
- Transformación digital en empresas manufactureras colombianas.

Estas fuentes permitirán complementar el análisis con información cuantitativa y tendencias del mercado.

## Segmentación del mercado

El mercado potencial de Polygon puede segmentarse principalmente hacia organizaciones que:

- Manejan procesos de producción por etapas.
- Utilizan Órdenes de Producción.
- Requieren trazabilidad entre diferentes áreas.
- Necesitan registrar información durante cada etapa productiva.
- Buscan disminuir la dependencia de formatos físicos.
- Necesitan consultar el estado de producción de manera centralizada.

El cliente principal de la primera versión de Polygon es Sumicolor.

## Clientes y usuarios objetivo

Dentro de Sumicolor existen diferentes perfiles que interactuarán con Polygon.

### Administrador

Necesita gestionar usuarios, roles, permisos y configuraciones necesarias para el funcionamiento del sistema.

### Personal de Planeación

Necesita crear, modificar, consultar y liberar las Órdenes de Producción.

### Operarios de producción

Necesitan consultar las órdenes asignadas a su etapa y registrar la información correspondiente al trabajo realizado.

Los principales operarios contemplados son:

- Picky
- Pesaje
- Mezcla
- Extrusión
- Empaque

### Inspector de Calidad

Necesita registrar los resultados de las inspecciones y determinar si una Orden de Producción puede continuar o debe entrar en reproceso.

### Responsables de producción

Necesitan consultar el estado, historial y posteriormente los reportes relacionados con las Órdenes de Producción.

---

# 5.3 Producto o Servicio

## Descripción del producto

Polygon es una aplicación web para gestionar digitalmente las Órdenes de Producción de Sumicolor.

La plataforma representa las diferentes etapas del proceso productivo y permite que la información de cada Orden de Producción avance de manera controlada entre ellas.

Entre las principales funcionalidades planteadas para Polygon se encuentran:

- Autenticación de usuarios.
- Control de acceso mediante roles.
- Administración de usuarios.
- Creación de Órdenes de Producción.
- Consulta y modificación de órdenes.
- Gestión de estados.
- Registro de información en cada etapa productiva.
- Control de calidad.
- Gestión de reprocesos.
- Empaque y finalización.
- Consulta de trazabilidad.
- Generación de reportes.

Cada registro realizado permanece asociado a la Orden de Producción correspondiente, permitiendo mantener la información organizada durante todo su ciclo de vida.

## Propuesta de Valor

Polygon busca proporcionar a Sumicolor una plataforma única para gestionar el flujo de sus Órdenes de Producción.

La propuesta de valor se basa en:

- Centralización de la información productiva.
- Trazabilidad de las Órdenes de Producción.
- Reducción de la dependencia de formatos físicos.
- Control de acceso según responsabilidades.
- Consulta del estado actual de una OP.
- Conservación del historial del proceso.
- Mayor disponibilidad de la información.
- Base de información para generar posteriormente reportes e indicadores.

Polygon no busca únicamente reemplazar un formato físico, sino conectar las diferentes etapas del proceso productivo dentro de un mismo flujo digital.

---

# 6. Calidad del Software

Durante el Sprint 1 se definirán estándares técnicos que deberán ser utilizados por todos los integrantes del proyecto.

## 6.1 Estándares de nombramiento

### Backend - Python / Django

Se propone seguir las convenciones establecidas por PEP 8.

- Variables: `snake_case`
- Funciones: `snake_case`
- Métodos: `snake_case`
- Clases: `PascalCase`
- Constantes: `UPPER_SNAKE_CASE`

Ejemplos:

```python
orden_produccion = ...
registrar_mezcla()
class OrdenProduccion:
    pass
MAX_INTENTOS = 3
```

### Frontend - Vue / JavaScript

Se utilizará una convención consistente para todo el frontend:

- Variables: `camelCase`
- Funciones: `camelCase`
- Componentes Vue: `PascalCase`
- Constantes: `UPPER_SNAKE_CASE`

Ejemplo:

```javascript
const ordenProduccion = {}
function registrarMezcla() {}
const API_BASE_URL = ''
```

Componente:

```text
OrdenProduccionForm.vue
```

---

# 6.2 Análisis estático de código

Como propuesta inicial para el proyecto se consideran las siguientes herramientas:

### Backend

**Ruff**

Permitirá detectar problemas de estilo, errores comunes y aplicar automáticamente diferentes reglas de formato para Python.

### Frontend

**ESLint**

Permitirá analizar estáticamente el código JavaScript/Vue y detectar errores o incumplimientos de los estándares definidos.

**Prettier**

Se utilizará como herramienta de formateo automático para mantener un estilo homogéneo en el código frontend.

La configuración definitiva de estas herramientas será acordada por el equipo.

---

# 6.3 Estrategia de ramas

El equipo utilizará una estrategia basada en GitHub Flow.

La rama principal será:

```text
main
```

Cada nueva funcionalidad se desarrollará en una rama independiente.

Convención:

```text
feature/hu-XX-descripcion
```

Ejemplos:

```text
feature/hu-01-login
feature/hu-10-crear-op
feature/hu-22-23-mezcla
```

El flujo de trabajo será:

```text
main
  ↓
feature/hu-XX
  ↓
desarrollo
  ↓
commit
  ↓
Pull Request
  ↓
revisión
  ↓
merge a main
```

El objetivo es evitar modificaciones directas sobre `main` y permitir que los cambios sean revisados antes de integrarse.

---

# 6.4 Integración Continua

Como mejora de calidad se contempla la utilización de GitHub Actions para ejecutar automáticamente comprobaciones sobre los Pull Requests.

El pipeline podrá incluir:

### Backend

```text
Instalar dependencias
→ Ejecutar Ruff
→ Ejecutar pruebas de Django
```

### Frontend

```text
npm ci
→ Ejecutar ESLint
→ Compilar aplicación con Vite
```

Un Pull Request podrá considerarse listo para integrar cuando las comprobaciones automáticas terminen correctamente.

La implementación del pipeline se realizará después de definir definitivamente las herramientas de calidad utilizadas por el equipo.

---

# 7. Pruebas Funcionales Manuales

Las pruebas funcionales estarán directamente relacionadas con las Historias de Usuario implementadas durante el Sprint 1.

Cada Historia de Usuario deberá conservar la siguiente relación:

```text
Historia de Usuario
        ↓
Criterio de aceptación
        ↓
Caso de prueba
        ↓
Ejecución
        ↓
Resultado
        ↓
Bug (si se encuentra un error)
```

Los criterios de aceptación serán ajustados al formato:

> Dada una situación, cuando ocurre determinada acción, entonces el sistema debe producir el resultado esperado.

## Estado

Diseño de casos de prueba: Pendiente de completar conforme se implementen las HU.

Ejecución de casos de prueba: Pendiente.

Reporte de bugs: Se registrarán como Issues cuando sean encontrados durante la ejecución.

---

# 8. Gestión del Backlog

Las Historias de Usuario correspondientes al Sprint serán administradas mediante GitHub Issues y el tablero del proyecto.

Los estados utilizados permitirán identificar como mínimo:

```text
Backlog
↓
Por hacer
↓
En progreso
↓
Revisión
↓
Finalizado
```

Una Historia de Usuario únicamente será considerada finalizada cuando:

- Su funcionalidad esté implementada.
- Cumpla los criterios de aceptación.
- Los casos de prueba correspondientes hayan sido ejecutados.
- Los errores críticos encontrados hayan sido solucionados.

---

# 9. Ceremonias del Sprint

Durante el Sprint se conservarán evidencias de las ceremonias realizadas por el equipo.

| Ceremonia | Objetivo | Evidencia |
|-----------|----------|-----------|
| Sprint Planning | Definir objetivo y alcance del Sprint | Pendiente |
| Daily / reuniones de seguimiento | Revisar avances y bloqueos | Pendiente |
| Sprint Review | Revisar las funcionalidades desarrolladas | Pendiente |
| Retrospectiva | Analizar aspectos positivos y oportunidades de mejora | Pendiente |

Las evidencias pueden incluir:

- Actas.
- Fotografías.
- Grabaciones.
- Capturas de reuniones.
- Registro de decisiones realizadas por el equipo.

---

# 10. Estado de los Entregables

| Entregable | Estado |
|------------|--------|
| MVP V1 | En desarrollo |
| Caso de Negocio V1 | En elaboración |
| Pruebas Funcionales Manuales | Pendiente de ejecución |
| Calidad del Software | En definición |
| Backlog actualizado | En progreso |
| Evidencias de ceremonias | En recopilación |
| Presentación Sprint 1 | Pendiente |

---

# 11. Resultado esperado del Sprint

Al finalizar el Sprint 1 se espera contar con una primera versión demostrable de Polygon en la que un usuario autorizado pueda ingresar al sistema y gestionar el inicio del ciclo de una Orden de Producción.

El resultado de este Sprint constituirá la base para que durante el Sprint 2 se implementen las etapas operativas de Picky, Pesaje, Mezcla y Extrusión.
