# Sprint 1 - Proyecto 2 - MVP Versión 1

## 1. Información general

**Proyecto:** Polygon  
**Cliente:** Empresa Sumicolor S.A. 
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

Sumicolor es una empresa manufacturera cuyo proceso productivo actualmente depende
de formatos físicos (lápiz y papel) para registrar la información generada en cada
etapa de una Orden de Producción: Planeación, Picky, Pesaje, Mezcla, Extrusión,
Control de Calidad y Empaque. Esta dependencia de registros físicos dificulta
mantener una trazabilidad centralizada y actualizada del estado de cada orden a
medida que avanza por las distintas áreas de la empresa.

Polygon nace como la solución de software encargada de digitalizar y centralizar
este proceso: reemplaza los formatos físicos por un flujo digital único, en el que
cada área registra la información correspondiente a su etapa y cualquier
responsable autorizado puede consultar el estado y el historial de una Orden de
Producción en tiempo real.

## Misión

Digitalizar y centralizar la gestión de las Órdenes de Producción de
Sumicolor mediante una plataforma que permita registrar, consultar y mantener la
trazabilidad de la información generada durante el proceso productivo.

## Visión

Consolidar Polygon como la plataforma central para la gestión del
proceso productivo de Sumicolor, facilitando el seguimiento de las Órdenes de
Producción, reduciendo la dependencia de registros físicos y mejorando la
disponibilidad y trazabilidad de la información en todas las etapas del proceso.

La misión y la visión son coherentes con la propuesta de valor de Polygon: no se
trata simplemente de reemplazar un formato físico por uno digital, sino de conectar
las distintas etapas del proceso productivo dentro de un mismo flujo, con control
de acceso por rol y trazabilidad completa de cada Orden de Producción desde que se
crea hasta que finaliza.

---

# 5.2 Análisis del Mercado

## Investigación de mercado

Colombia se encuentra en una etapa de adopción digital todavía desigual pero en aceleración.
Según el Índice Global de Madurez de Transformación Digital 2023 (Cintel), el 55% de las
empresas del país está en fase de planificación de su estrategia digital y solo un 7% la
tiene completamente implementada [1]. En respuesta a este panorama, el Ministerio TIC
definió la Estrategia Nacional Digital 2023-2026, cuyo objetivo es fortalecer el sector
productivo colombiano mediante el uso y apropiación de datos y tecnologías digitales [2].

Dentro de este panorama general, la industria manufacturera colombiana muestra una madurez
digital superior a la de otros sectores productivos: un estudio reciente enfocado en
proyectos de adopción tecnológica encontró que solo el 9% de las empresas manufactureras
colombianas está en una etapa "Nula" de digitalización, frente a un 45% que ya se considera
avanzado en el uso de datos y tecnología en sus procesos [3]. Esto es consistente con
reportes internacionales sobre Industria 4.0: McKinsey estima que la automatización de
procesos productivos puede aumentar la productividad hasta en un 40%, Deloitte reporta
reducciones de costos operativos de hasta un 20%, PwC señala una disminución de errores de
producción del 80%, y Forrester indica incrementos de hasta un 50% en capacidad de
producción [4]. Aun así, Colombia ocupa todavía una posición intermedia frente al resto de
Latinoamérica en adopción de Industria 4.0 [4], lo que sugiere que buena parte del mercado
manufacturero nacional -incluidas empresas del tamaño de Sumicolor- todavía opera con
procesos manuales o parcialmente digitalizados, exactamente el problema que Polygon busca
resolver.

En términos de categoría de producto, Polygon se ubica dentro de lo que la industria conoce
como MES (Manufacturing Execution System): un software que monitorea, controla, rastrea y
documenta los procesos de manufactura en el piso de producción en tiempo real, cerrando la
brecha entre los sistemas de planeación empresarial y la ejecución física de la producción,
y garantizando trazabilidad completa desde la materia prima hasta el producto terminado [5].
Polygon corresponde a una versión enfocada de este tipo de solución: en lugar de cubrir toda
la operación de una planta industrial grande, se concentra específicamente en la
trazabilidad de la Orden de Producción de Sumicolor a través de sus etapas (Planeación,
Picky, Pesaje, Mezcla, Extrusión, Calidad y Empaque).

## Fuentes:

1. Cintel — Índice Global de Madurez de Transformación Digital 2023, citado en *"Así se
   perfila la transformación para las empresas en 2025"*, Claro Empresas (28 may. 2025).
   https://www.claro.com.co/empresas/noticias-interes/transformacion-empresas-2025/
2. MinTIC — *Estrategia Nacional Digital de Colombia 2023-2026*.
   https://www.mintic.gov.co/portal/715/articles-334120_recurso_1.pdf
3. *Análisis del principal desafío que enfrentan las industrias del sector Ingeniería,
   Industria Manufacturera y Agricultura en Colombia para adoptar tecnologías digitales...*,
   repositorio institucional UNIMINUTO.
   https://repository.uniminuto.edu/items/237e0e1a-279b-4b5e-97d3-2ab0c4aa0fc5
4. *"Industria 4.0: La apuesta que Colombia no puede seguir ignorando"*, Revista LatinPyme.
   https://latinpyme.com/blog/revista-latinpyme-2/industria-4-0-la-apuesta-que-colombia-no-puede-seguir-ignorando-888
5. Tractian — Glosario, *"MES (Sistema de Ejecución de Manufactura)"*.
   https://tractian.com/es/glosario/mes-manufacturing-execution-system

## Segmentación del mercado

El mercado potencial de Polygon se segmenta hacia organizaciones que:

- Manejan procesos de producción por etapas secuenciales.
- Utilizan Órdenes de Producción como unidad de control del proceso productivo.
- Requieren trazabilidad entre diferentes áreas o departamentos de planta.
- Necesitan registrar información específica durante cada etapa productiva.
- Buscan reducir su dependencia de formatos físicos (papel) para el control de producción.
- Necesitan consultar el estado de producción de manera centralizada y en tiempo real.

Este segmento corresponde, en términos de la industria, a pequeñas y medianas empresas
manufactureras que aún no cuentan con un sistema MES o ERP de producción, y que dependen de
procesos manuales para su trazabilidad — precisamente el segmento donde, según los datos de
Cintel citados arriba, todavía se concentra la mayoría de la industria colombiana [1].

El cliente principal de la primera versión de Polygon es **Sumicolor**.

## Clientes y usuarios objetivo

Dentro de Sumicolor existen distintos perfiles que interactúan con Polygon:

**Administrador.** Gestiona usuarios, roles, permisos y las configuraciones necesarias para
el funcionamiento del sistema.

**Personal de Planeación.** Crea, modifica, consulta y libera las Órdenes de Producción hacia
la siguiente etapa del proceso.

**Operarios de producción.** Consultan las órdenes asignadas a su etapa y registran la
información correspondiente al trabajo realizado. Los operarios contemplados son: Picky,
Pesaje, Mezcla, Extrusión y Empaque.

**Inspector de Calidad.** Registra los resultados de las inspecciones y determina si una
Orden de Producción puede continuar o debe entrar en reproceso.

**Responsables de producción.** Consultan el estado, historial y, más adelante, los reportes
relacionados con las Órdenes de Producción.

---

# 5.3 Producto o Servicio

## Descripción del producto

Polygon es una aplicación web para gestionar digitalmente las Órdenes de Producción de
Sumicolor. La plataforma representa las diferentes etapas del proceso productivo y permite
que la información de cada Orden de Producción avance de manera controlada entre ellas,
reemplazando los formatos físicos que Sumicolor utiliza actualmente para registrar esta
información.

En este MVP V1 (Sprint 1), Polygon ya cubre de forma funcional el ciclo completo de una
Orden de Producción desde su creación hasta su ingreso al proceso de Mezcla:

- Autenticación de usuarios y control de acceso según su rol (administrador, personal de
  Planeación, operario de Picky, operario de Pesaje).
- Administración de usuarios: creación, edición de datos y asignación de roles, con registro
  de auditoría de cada cambio realizado.
- Creación de Órdenes de Producción, con número de orden único autogenerado y validación de
  campos obligatorios.
- Edición y consulta de Órdenes de Producción mientras se encuentran en estado Planeación.
- Liberación de una Orden de Producción hacia Picky, validando que la información esté
  completa antes de permitir el cambio de estado.
- Consulta y registro de materiales preparados en Picky, con confirmación de envío hacia
  Pesaje.
- Consulta y registro del pesaje de materias primas en Pesaje, con confirmación de envío
  hacia Mezcla.

Cada registro realizado permanece asociado a la Orden de Producción correspondiente,
permitiendo mantener la información organizada y trazable durante todo su recorrido por las
distintas áreas de la planta.

Quedan para los siguientes Sprints las etapas operativas de Mezcla, Extrusión, Control de
Calidad y Empaque, así como la administración de catálogos (productos, materias primas,
clientes) y la generación de reportes.

## Propuesta de Valor

Polygon busca proporcionar a Sumicolor una plataforma única para gestionar el flujo de sus
Órdenes de Producción. La propuesta de valor se basa en:

- Centralización de la información productiva, eliminando el uso de formatos físicos en las
  etapas ya cubiertas por el sistema (Planeación, Picky y Pesaje).
- Trazabilidad de la Orden de Producción: cada cambio de estado y cada dato registrado queda
  asociado a la orden y a la persona que lo registró.
- Control de acceso según responsabilidades: cada rol solo ve y modifica la información que
  le corresponde.
- Consulta del estado actual de una OP en cualquier momento, sin depender de reportes
  verbales o físicos entre áreas.
- Conservación del historial de cambios de usuarios y de Órdenes de Producción, como base
  para auditoría.
- Base de información estructurada para, en Sprints posteriores, generar reportes e
  indicadores de producción.

Polygon no busca únicamente reemplazar un formato físico, sino conectar las diferentes etapas
del proceso productivo dentro de un mismo flujo digital, empezando por el tramo
Planeación → Picky → Pesaje que ya es funcional en este MVP.

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

### Frontend - Vue / JavaScript

Se utilizará una convención consistente para todo el frontend:

- Variables: `camelCase`
- Funciones: `camelCase`
- Componentes Vue: `PascalCase`
- Constantes: `UPPER_SNAKE_CASE`


---

## 6.2 Análisis estático de código

Como propuesta inicial para el proyecto se consideran las siguientes herramientas:

### Backend

**Ruff**

Permitirá detectar problemas de estilo, errores comunes y aplicar automáticamente diferentes reglas de formato para Python.

### Frontend

**ESLint**

Permitirá analizar estáticamente el código JavaScript/Vue y detectar errores o incumplimientos de los estándares definidos.

**Prettier**

Se utilizará como herramienta de formateo automático para mantener un estilo homogéneo en el código frontend.

---

## 6.3 Estrategia de ramas

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

## 6.4 Integración Continua

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



---

# 10. Resultado esperado del Sprint

Al finalizar el Sprint 1 se espera contar con una primera versión demostrable de Polygon en la que un usuario autorizado pueda ingresar al sistema y gestionar el inicio del ciclo de una Orden de Producción.

El resultado de este Sprint constituirá la base para que durante el Sprint 2 se implementen las etapas operativas de Picky, Pesaje, Mezcla y Extrusión.
