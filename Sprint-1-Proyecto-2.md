# Sprint 1 - Proyecto 2 - MVP Versión 1

# 1. Información general

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

## 6.1 Estándares de nombramiento

### Backend — Python / Django

El backend sigue las convenciones de **PEP 8**, el estándar oficial de nombramiento y estilo
del lenguaje Python [1]:

| Elemento | Convención | Ejemplo real del proyecto |
| --- | --- | --- |
| Variables | `snake_case` | `numero_orden`, `creado_por` |
| Funciones y métodos | `snake_case` | `_generar_numero_orden()`, `perform_update()` |
| Clases | `PascalCase` | `OrdenProduccion`, `RegistroAuditoriaUsuario` |
| Constantes | `UPPER_SNAKE_CASE` | `CAMPOS_AUDITADOS` |

**Justificación.** PEP 8 se eligió porque es el estándar oficial del lenguaje —definido por
la Python Software Foundation— y es el que sigue Django internamente en su propio código
fuente y en su documentación, por lo que mantiene el proyecto consistente con el framework
que se está usando.

### Frontend — Vue 3 / JavaScript

| Elemento | Convención | Ejemplo real del proyecto |
| --- | --- | --- |
| Variables y funciones | `camelCase` | `guardarEdicion()`, `erroresCrear` |
| Componentes Vue | `PascalCase` | `GestionUsuarios.vue`, `OrdenesProduccion.vue` |
| Constantes | `UPPER_SNAKE_CASE` | `ESTADO_BADGE` |

**Justificación.** El nombramiento de componentes en `PascalCase` sigue la [Guía de Estilo
Oficial de Vue.js][2], que lo marca como **fuertemente recomendado** ("strongly
recommended"), precisamente para evitar ambigüedad con las etiquetas nativas de HTML (que
son en minúsculas). El `camelCase` para variables y funciones sigue la convención estándar
de JavaScript, tal como la documenta la [Guía de Estilo de JavaScript de Airbnb][3], una de
las referencias más adoptadas por la comunidad para proyectos JS/Vue.

#### Nombramiento enforzado por herramientas (no solo documentado)

Ambos estándares ya están configurados para bloquear el código que no los cumpla, no
dependen de que cada desarrollador se acuerde de seguirlos:

- **Backend:** en `pyproject.toml`, Ruff tiene habilitado el grupo de reglas `"N"`
  (`pep8-naming`), que revisa automáticamente que clases estén en `PascalCase` y
  funciones/variables en `snake_case`.
- **Frontend:** en `eslint.config.js`, `pluginVue.configs['flat/recommended']` incluye la
  regla `vue/component-definition-name-casing` (fuerza `PascalCase` en componentes), y se
  agregó explícitamente la regla `camelcase` para variables y funciones JS.
- **Formateo automático:** Prettier (`.prettierrc.json`) corrige el estilo del código
  frontend automáticamente al guardar, y Ruff (`ruff format`) hace lo mismo en el backend.

**Referencias**

1. Python Software Foundation — *PEP 8: Style Guide for Python Code*.
   https://peps.python.org/pep-0008/
2. Vue.js — *Guía de Estilo Oficial* (Component name casing in templates: strongly
   recommended). https://vuejs.org/style-guide/
3. Airbnb — *JavaScript Style Guide*. https://github.com/airbnb/javascript

- Evidencia por medio del comando ruff check . en el backend

<img width="1917" height="977" alt="image" src="https://github.com/user-attachments/assets/9c220ba4-b7fc-44be-90b6-688c5e3afdcf" />

- Evidencia por medio del comando npx eslint . en el frontend

<img width="1917" height="980" alt="image" src="https://github.com/user-attachments/assets/b8fe4542-d0d1-4035-bf6d-1bd2c85865eb" />

---

## 6.2 Análisis estático de código

El proyecto usa dos herramientas, una por cada lenguaje del stack, ambas ya integradas y
configuradas:

### Backend: Ruff

**Qué es y por qué se eligió.** Ruff es un linter y formateador de código Python escrito en
Rust [1]. Se eligió sobre alternativas más tradicionales (flake8 + isort + black por
separado) porque combina en una sola herramienta y un solo archivo de configuración las
reglas de estilo (pycodestyle), errores lógicos (pyflakes), orden de imports (isort),
convenciones de nombres (pep8-naming), sintaxis moderna (pyupgrade), patrones propensos a
bugs (flake8-bugbear) y buenas prácticas específicas de Django (flake8-django), además de
formatear el código automáticamente — todo eso sin tener que instalar ni sincronizar
configuraciones entre varias herramientas distintas.

**Configuración (`Backend/pyproject.toml`):**

```toml
[tool.ruff]
line-length = 100
target-version = "py312"
extend-exclude = ["migrations"]

[tool.ruff.lint]
select = [
    "E",   # pycodestyle (errores)
    "W",   # pycodestyle (warnings)
    "F",   # pyflakes (errores logicos: imports/variables sin usar, etc.)
    "I",   # isort (orden de imports)
    "N",   # pep8-naming (convenciones de nombres: clases, funciones, variables)
    "UP",  # pyupgrade (sintaxis moderna de Python)
    "B",   # flake8-bugbear (patrones propensos a bugs)
    "C4",  # flake8-comprehensions (comprehensions mas claras/eficientes)
    "SIM", # flake8-simplify (simplificacion de codigo)
    "DJ",  # flake8-django (buenas practicas especificas de Django)
]
ignore = [
    "DJ001", # permite null=True en CharField/TextField (uso comun para campos opcionales)
]

[tool.ruff.lint.isort]
known-first-party = ["config", "apps"]

[tool.ruff.format]
quote-style = "single"
```

**Justificación de esta configuración.** El conjunto de reglas seleccionado va más allá del
modo por defecto de Ruff (que solo trae `E` y `F`): se optó deliberadamente por un modo más
estricto, cercano al recomendado por la comunidad para proyectos Django, agregando `N`
(nombramiento, ver sección 6.1), `B` (errores comunes que no son sintácticos pero sí
riesgosos) y `DJ` (buenas prácticas propias del framework). Se ignora únicamente `DJ001`
porque el equipo decidió permitir `null=True` en campos de texto opcionales
(`observaciones`, por ejemplo), un caso de uso legítimo y frecuente en este proyecto.
`exclude = ["migrations"]` evita que Ruff revise archivos autogenerados por Django, que no
tiene sentido "corregir" a mano.

**Ejecución:** `ruff check .` (revisión) y `ruff format .` (corrección automática de
estilo). Corrido sobre el código de las apps `usuarios` y `produccion`, no reporta errores.

### Frontend: ESLint + Prettier

**Qué son y por qué se eligieron.** ESLint es el linter estándar de facto para JavaScript
[2], y `eslint-plugin-vue` es el plugin oficial mantenido por el equipo de Vue.js para
analizar archivos `.vue` [3]. Prettier es el formateador de código más usado en el
ecosistema JavaScript/Vue [4]. Se usan juntos porque cada uno cubre un problema distinto:
ESLint detecta errores y malas prácticas (variables sin usar, nombres de componentes mal
formados, etc.), y Prettier resuelve el formato/estilo visual (comillas, punto y coma,
indentación) — por eso al final de la configuración de ESLint se desactiva
`eslint-config-prettier`, para que las reglas de estilo de ambas herramientas no compitan
entre sí.

**Configuración (`Frontend/eslint.config.js`):**

```js
export default [
  { ignores: ['dist/**', 'node_modules/**'] },

  js.configs.recommended,
  ...pluginVue.configs['flat/recommended'],

  {
    rules: {
      camelcase: ['warn', { properties: 'never' }],
      'no-unused-vars': ['warn', { argsIgnorePattern: '^_' }],
      'vue/multi-word-component-names': 'off',
    },
  },

  configPrettier,
]
```

**Justificación de esta configuración.** `eslint-plugin-vue` ofrece tres niveles de estrictez
oficiales: `essential` (mínimo, evita errores), `strongly-recommended` (agrega estilo) y
`recommended` (el más estricto de los tres) [3]. El proyecto adoptó directamente
**`flat/recommended`**, el modo más estricto disponible, en línea con lo que sugiere el
documento maestro ("modo estricto o recomendado por la comunidad"). Se desactivó
puntualmente `vue/multi-word-component-names` porque el proyecto tiene componentes de una
sola palabra a propósito (`Badge.vue`, `Login.vue`) donde el riesgo que esa regla previene
—colisión con futuras etiquetas nativas de HTML— no aplica. `camelcase` se agregó de forma
explícita para reforzar la convención de nombres documentada en la sección 6.1.

**Configuración de Prettier (`Frontend/.prettierrc.json`):**

```json
{
  "semi": false,
  "singleQuote": true,
  "printWidth": 100,
  "trailingComma": "all",
  "arrowParens": "always"
}
```

**Ejecución:** `npx eslint .` (revisión) y Prettier se integra en el editor/al guardar para
formatear automáticamente. Corrido sobre `Frontend/src`, no reporta errores.

### Puntos adicionales cubiertos

- **Nombramiento enforzado por la herramienta:** sí, en ambos lados (`N` en Ruff,
  `vue/component-definition-name-casing` + `camelcase` en ESLint) — detalle completo en la
  sección 6.1.
- **Corrección automática de estilo (formateador):** sí, en ambos lados (`ruff format` en el
  backend, Prettier en el frontend).

**Referencias**

1. Astral — *Ruff: An extremely fast Python linter and code formatter*.
   https://docs.astral.sh/ruff/
2. ESLint — *Getting Started with ESLint*.
   https://eslint.org/docs/latest/use/getting-started
3. eslint-plugin-vue — *User Guide* (niveles essential / strongly-recommended /
   recommended). https://eslint.vuejs.org/user-guide/
4. Prettier — *Documentación oficial*. https://prettier.io/docs/en/index.html

---

## 6.3 Estrategia de ramas

El equipo utiliza **GitHub Flow** [1]: `main` es la rama principal y siempre debe quedar en
un estado desplegable; todo cambio se desarrolla en una rama de feature independiente, se
somete a Pull Request, se revisa y se fusiona a `main`.

```text
main
  ↓
feature/hu-XX-descripcion
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

Convención de nombres definida: `feature/hu-XX-descripcion`, agrupando varias HU en una
misma rama cuando forman parte de un mismo bloque funcional (ejemplo:
`feature/hu-22-23-mezcla`).

**Estado real del repositorio (8 ramas activas, verificado en GitHub):**

| Rama | ¿Sigue la convención? |
| --- | --- |
| `main` (default) | — |
| `feature/hu-01-02-04-login-logout-usuarios` | ✅ Sí |
| `feature/hu-05-06-10-11-usuario-roles-ord...` | ✅ Sí |
| `feature/hu-12-13-15-16-17-picky-produc...` | ✅ Sí |
| `feature/hu-18-consultar-pesaje` | ✅ Sí |
| `feature/hu-19-20-pesaje` | ✅ Sí |
| `feature/hu-05-06-10-11-correcciones` | ✅ Sí |
| `feature/hu-all-correcciones-generales` | ✅ Sí |

Git Flow

<img width="1132" height="697" alt="Captura de pantalla 2026-09-08 225858" src="https://github.com/user-attachments/assets/d956977e-336e-44f4-9b3f-d9f66f794a79" />

## 6.4 Integración Continua

**Estado actual: no implementado.** El repositorio todavía no tiene ningún workflow de
GitHub Actions configurado — no hay carpeta `.github/workflows/` en el proyecto. Esto es una
tarea pendiente, no algo que ya esté funcionando.

**Propuesta de pipeline** (a implementar), retomando las herramientas ya documentadas en
6.1/6.2, para que el análisis estático deje de depender de que cada persona lo corra
manualmente antes de hacer commit:

```yaml
# .github/workflows/ci.yml (propuesta, pendiente de crear)
name: CI

on:
  pull_request:
    branches: [main]

jobs:
  backend:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: Backend
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install -r requirements.txt
      - run: ruff check .
      - run: python manage.py test

  frontend:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: Frontend
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
      - run: npm ci
      - run: npx eslint .
      - run: npm run build
```

Este pipeline corre en cada Pull Request hacia `main` y ejecuta, en paralelo, la revisión de
estilo/nombramiento (`ruff check`, `eslint`) y una verificación funcional básica (`manage.py
test` en backend, `npm run build` para confirmar que el frontend compila). Para que cumpla lo
que pide el documento maestro ("bloquear la fusión de un PR hasta que el pipeline esté
'green' y tenga aprobación de revisión de pares"), falta además:

1. Crear el archivo de workflow de arriba en `.github/workflows/`.
2. Activar **branch protection** sobre `main` en la configuración del repositorio, marcando
   este workflow como *required status check* y exigiendo al menos una aprobación de PR.
3. (Pendiente aparte, fuera de este Sprint) escribir pruebas automatizadas de Django
   (`manage.py test`) — hoy el proyecto no tiene ninguna, así que ese paso del pipeline
   pasaría siempre, pero sin verificar nada real todavía.

**Referencias**

1. GitHub Docs — *GitHub flow*. https://docs.github.com/en/get-started/using-github/github-flow

---

# 7. Diseño de casos de Pruebas Funcionales

# 7.1 Casos de Prueba Funcionales — Sprint 1

Las pruebas funcionales estarán directamente relacionadas con las Historias de Usuario implementadas durante el Sprint 1.

> Formato: **Dada** una situación, **cuando** ocurre una acción, **entonces** el sistema
> produce el resultado esperado. Cada CP va asociado a su HU y a su criterio de aceptación
> en el Issue correspondiente de GitHub.

## HU-01 Iniciar sesión

**CP-01-01 (criterio: el usuario puede ingresar con sus credenciales / el sistema valida
que sean correctas)**
Dado un usuario activo con usuario y contraseña correctos,
cuando envía el formulario de login,
entonces el sistema lo autentica y responde con un token de sesión.

**CP-01-02 (criterio: si las credenciales son incorrectas se muestra un mensaje de error)**
Dado un usuario que ingresa una contraseña incorrecta,
cuando envía el formulario de login,
entonces el sistema rechaza el intento y muestra un mensaje de error, sin generar sesión.

**CP-01-03 (criterio: el sistema identifica el rol y redirige al módulo correspondiente)**
Dado un usuario autenticado con rol "admin",
cuando inicia sesión exitosamente,
entonces el sistema lo redirige al módulo de Usuarios; y dado un usuario con rol
"supervisor", entonces lo redirige al módulo de Órdenes de Producción.

**CP-01-04 (regla de negocio: solo usuarios activos pueden iniciar sesión)**
Dado un usuario existente marcado como inactivo,
cuando intenta iniciar sesión con sus credenciales correctas,
entonces el sistema rechaza el acceso.

---

## HU-02 Cerrar sesión

**CP-02-01 (criterio: el usuario puede cerrar sesión desde cualquier módulo)**
Dado un usuario con sesión activa en cualquier módulo del sistema,
cuando selecciona "Cerrar sesión",
entonces el sistema elimina la sesión activa y lo redirige a la pantalla de login.

**CP-02-02 (regla de negocio: ningún usuario puede seguir usando el sistema tras cerrar
sesión)**
Dado un usuario que acaba de cerrar sesión,
cuando intenta acceder de nuevo a una ruta protegida sin volver a iniciar sesión,
entonces el sistema le niega el acceso y lo redirige al login.

---

## HU-04 Crear usuario

**CP-04-01 (criterio: el administrador puede registrar un nuevo usuario, queda asociado a
un rol y activo)**
Dado un administrador autenticado que diligencia los datos de un nuevo usuario con un rol
válido,
cuando envía el formulario de creación,
entonces el sistema crea el usuario, lo asocia al rol indicado y lo deja activo para iniciar
sesión.

**CP-04-02 (criterio: el sistema valida que no existan usuarios duplicados)**
Dado que ya existe un usuario con un nombre de usuario determinado,
cuando el administrador intenta crear otro usuario con ese mismo nombre de usuario,
entonces el sistema rechaza la creación e informa el error.

**CP-04-03 (regla de negocio: solo un administrador puede crear usuarios)**
Dado un usuario autenticado con rol distinto a "admin" (supervisor o planta),
cuando intenta crear un nuevo usuario,
entonces el sistema rechaza la solicitud por falta de permisos.

**CP-04-04 (regla de negocio: cada usuario debe tener un rol asignado)**
Dado un administrador que intenta crear un usuario sin especificar un rol,
cuando envía el formulario,
entonces el sistema no permite guardar el usuario hasta que se indique un rol válido.

---

## HU-05 Editar usuario

**CP-05-01 (criterio: el administrador puede modificar la información de un usuario y el
sistema guarda los cambios)**
Dado un administrador autenticado y un usuario existente,
cuando edita su nombre, apellido, rol o estado y guarda los cambios,
entonces el sistema actualiza la información del usuario correctamente.

**CP-05-02 (criterio: el sistema mantiene un registro de la modificación)**
Dado un administrador que edita el rol de un usuario de "planta" a "supervisor",
cuando guarda el cambio,
entonces el sistema registra en el historial de auditoría el campo modificado, el valor
anterior, el valor nuevo y quién hizo el cambio.

**CP-05-03 (regla de negocio: solo el administrador puede modificar usuarios)**
Dado un usuario autenticado con rol "supervisor" o "planta",
cuando intenta editar los datos de otro usuario,
entonces el sistema rechaza la solicitud por falta de permisos.

---

## HU-06 Asignar roles y permisos

**CP-06-01 (criterio: el administrador puede asignar un rol y el sistema aplica los
permisos correspondientes)**
Dado un administrador que cambia el rol de un usuario a "supervisor",
cuando el usuario afectado vuelve a iniciar sesión,
entonces el sistema le concede acceso al módulo de Órdenes de Producción, correspondiente a
su nuevo rol.

**CP-06-02 (criterio: los usuarios solo visualizan los módulos autorizados)**
Dado un usuario con rol "planta",
cuando intenta acceder directamente a la URL del módulo de Usuarios o de Órdenes de
Producción,
entonces el sistema le niega el acceso, sin importar que conozca la ruta.

**CP-06-03 (regla de negocio: cada usuario debe tener un único rol principal)**
Dado un usuario existente con rol "planta",
cuando el administrador le asigna el rol "supervisor",
entonces el sistema reemplaza el rol anterior por el nuevo, sin que el usuario quede con más
de un rol activo a la vez.

---

## HU-10 Crear Orden de Producción

**CP-10-01 (criterio: se puede crear una nueva OP y el sistema asigna un número único)**
Dado un usuario de Planeación autenticado que diligencia producto, cantidad, materia prima
y observaciones,
cuando guarda la nueva Orden de Producción,
entonces el sistema la crea con un número de orden único generado automáticamente y estado
"Planeación".

**CP-10-02 (criterio: todos los campos obligatorios son validados)**
Dado un usuario de Planeación que intenta crear una OP dejando vacío el campo "producto" o
"materia prima", o con cantidad igual o menor a cero,
cuando envía el formulario,
entonces el sistema rechaza la creación e informa cuál campo es inválido.

**CP-10-03 (criterio: la orden queda disponible para continuar hacia Picky)**
Dado una OP recién creada en estado "Planeación",
cuando se consulta su estado,
entonces figura como disponible para ser liberada hacia Picky.

**CP-10-04 (regla de negocio: no pueden existir dos órdenes con el mismo número)**
Dado que ya existen Órdenes de Producción creadas previamente,
cuando se crea una nueva OP,
entonces el número de orden asignado por el sistema es distinto a los de todas las órdenes
existentes.

---

## HU-11 Editar Orden de Producción

**CP-11-01 (criterio: se puede modificar una OP en estado Planeación y el sistema guarda los
cambios)**
Dado una OP en estado "Planeación",
cuando el usuario de Planeación edita su cantidad, producto, materia prima, urgencia u
observaciones y guarda,
entonces el sistema actualiza la información correctamente.

**CP-11-02 (criterio: el sistema registra fecha, hora y usuario de la modificación)**
Dado una OP en estado "Planeación" que es editada,
cuando se guarda el cambio,
entonces el sistema registra en el historial el campo modificado, los valores anterior y
nuevo, quién hizo el cambio y cuándo.

**CP-11-03 (criterio y regla de negocio: no es posible modificar una OP que ya inició
producción / solo editable antes de liberarse a Picky)**
Dado una OP que ya fue liberada y se encuentra en estado "Picky" (o posterior),
cuando el usuario de Planeación intenta editarla,
entonces el sistema rechaza la modificación indicando que la OP ya no está en Planeación.

---

## HU-12 Consultar Órdenes de Producción

**CP-12-01 (criterio: el usuario puede consultar el listado de OP y ver su estado actual)**
Dado un usuario autorizado (admin o supervisor) autenticado,
cuando accede al listado de Órdenes de Producción,
entonces el sistema muestra todas las OP con su información básica y su estado actual.

**CP-12-02 (criterio: el usuario puede ingresar al detalle de una OP)**
Dado un listado de Órdenes de Producción visible,
cuando el usuario selecciona una OP específica,
entonces el sistema muestra el detalle completo de esa orden.

**CP-12-03 (regla de negocio: solo los usuarios autorizados pueden consultar órdenes)**
Dado un usuario con rol "planta" sin acceso al módulo de Órdenes de Producción general,
cuando intenta consultar el listado completo de OP,
entonces el sistema le niega el acceso.

---

## HU-13 Liberar Orden de Producción a Picky

**CP-13-01 (criterio: la OP cambia al estado "Picky" y queda disponible para ese personal)**
Dado una OP en estado "Planeación" con todos los campos obligatorios completos,
cuando el usuario de Planeación la libera hacia Picky,
entonces el sistema cambia su estado a "Picky" y queda visible para el personal de esa
etapa.

**CP-13-02 (criterio y regla de negocio: si existen campos pendientes el sistema impide
continuar / no puede liberarse una OP incompleta)**
Dado una OP en estado "Planeación" con campos obligatorios incompletos,
cuando el usuario intenta liberarla hacia Picky,
entonces el sistema impide el cambio de estado e informa qué falta completar.

---

## HU-15 Consultar Orden de Producción en Picky

**CP-15-01 (criterio: el operario puede visualizar las OP pendientes y solo las de Picky)**
Dado que existen Órdenes de Producción en distintos estados (Planeación, Picky, Pesaje,
etc.),
cuando un operario de Picky consulta su listado de órdenes pendientes,
entonces el sistema muestra únicamente las OP que están en estado "Picky".

**CP-15-02 (criterio: se muestra la información necesaria para preparar los materiales)**
Dado una OP en estado "Picky",
cuando el operario consulta su detalle,
entonces el sistema muestra el producto, la cantidad y la materia prima requerida para
prepararla.

---

## HU-16 Registrar materiales preparados

**CP-16-01 (criterio: el operario puede registrar los materiales preparados y quedan
asociados a la OP)**
Dado una OP en estado "Picky" que el operario está atendiendo,
cuando registra los materiales preparados para esa orden,
entonces el sistema almacena la información y la asocia correctamente a esa OP.

---

## HU-17 Confirmar envío a Pesaje

**CP-17-01 (criterio: el sistema valida los datos y la OP cambia al estado Pesaje)**
Dado una OP en Picky con los materiales preparados ya registrados completamente,
cuando el operario confirma el envío a Pesaje,
entonces el sistema cambia el estado de la OP a "Pesaje".

**CP-17-02 (criterio: si existen datos pendientes el sistema impide continuar)**
Dado una OP en Picky con información de materiales preparados incompleta,
cuando el operario intenta confirmar el envío a Pesaje,
entonces el sistema impide el cambio de estado e informa qué falta.

---

## HU-18 Consultar Orden de Producción en Pesaje

**CP-18-01 (criterio: el operario visualiza solo las OP pendientes en Pesaje)**
Dado que existen Órdenes de Producción en distintos estados,
cuando un operario de Pesaje consulta su listado de órdenes pendientes,
entonces el sistema muestra únicamente las OP en estado "Pesaje".

**CP-18-02 (criterio: se visualiza la información necesaria para realizar el pesaje)**
Dado una OP en estado "Pesaje",
cuando el operario consulta su detalle,
entonces el sistema muestra la información necesaria para pesar las materias primas
correspondientes.

---

## HU-19 Registrar pesaje de materias primas

**CP-19-01 (criterio: el operario puede registrar el pesaje y queda asociado a la OP)**
Dado una OP en estado "Pesaje" que el operario está atendiendo,
cuando registra las cantidades pesadas de las materias primas,
entonces el sistema almacena la información y la asocia correctamente a esa OP.

---

## HU-20 Confirmar envío a Mezcla

**CP-20-01 (criterio: el sistema valida la información y la OP cambia al estado Mezcla)**
Dado una OP en Pesaje con la información de pesaje registrada completamente,
cuando el operario confirma el envío a Mezcla,
entonces el sistema cambia el estado de la OP a "Mezcla".

**CP-20-02 (criterio: el sistema impide avanzar si existen datos pendientes)**
Dado una OP en Pesaje con información de pesaje incompleta,
cuando el operario intenta confirmar el envío a Mezcla,
entonces el sistema impide el cambio de estado e informa qué falta.

---

# 7.2 Ejecución de Pruebas y Reporte de Errores

## Evidencia de algunas pruebas realizadas, la la ejecución total del flujo del desarrollo del Sprint 1 se verá evidenciado en la sustentación:

### Inicio de sesión

<img width="1917" height="872" alt="image" src="https://github.com/user-attachments/assets/6904f2ff-d1ac-48a8-9bae-5de243da3adc" />

### Vista del Administrador

<img width="1917" height="866" alt="image" src="https://github.com/user-attachments/assets/ed0ff888-4803-4aa8-a37f-4addab685bc9" />

### Nuevo usuario y asignar rol

<img width="1917" height="875" alt="image" src="https://github.com/user-attachments/assets/d5d00d5e-0930-4396-b3b6-fad62b9ef1b6" />

## Evidencia de un BUG reportado

<img width="1282" height="651" alt="Captura de pantalla 2026-09-08 214459" src="https://github.com/user-attachments/assets/d7e3905d-d04c-4ad1-8137-f69d57ca07dd" />

---

# 7.3 Conexión HUCP-Errores

Cada Historia de Usuario deberá conservar la siguiente relación lógica:

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

---

# 8. Gestión del Backlog

Las Historias de Usuario del Sprint se administraron mediante GitHub Issues y el tablero
del proyecto (GitHub Projects), usando como mínimo los siguientes estados:

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

Una Historia de Usuario solo se consideró finalizada cuando:

- Su funcionalidad quedó implementada.
- Cumplió los criterios de aceptación definidos.
- Los casos de prueba correspondientes fueron ejecutados (ver sección "Diseño de casos de
  Pruebas Funcionales").
- Los errores críticos encontrados durante la ejecución de pruebas fueron solucionados.

**Estado al cierre del Sprint 1.** Las 15 Historias de Usuario comprometidas para este
Sprint (HU-01, HU-02, HU-04, HU-05, HU-06, HU-10, HU-11, HU-12, HU-13, HU-15, HU-16, HU-17,
HU-18, HU-19 y HU-20) llegaron al estado "Finalizado" en el tablero del proyecto.

---

# 9. Ceremonias del Sprint

Durante el Sprint 1 el equipo realizó las cuatro ceremonias de Scrum: Sprint Planning,
Daily Scrum, Sprint Review y Retrospectiva. A continuación se documenta cada una con su
respectiva evidencia (actas, grabaciones y capturas).

## Sprint Planning

**Objetivo.** Definir el Sprint Goal y seleccionar las Historias de Usuario del backlog que el equipo se comprometió a entregar en el Sprint 1.

| Campo | Detalle |
| --- | --- |
| Fecha | 4 de agosto |
| Participantes | equipo Polygon |
| Acuerdos / HU seleccionadas | Sprint Goal ya definido en la sección 2 de este documento |

## Daily Scrum

**Objetivo.** Sincronizar al equipo diariamente sobre avances, bloqueos y próximos pasos
durante la ejecución del Sprint.

| Campo | Detalle |
| --- | --- |
| Frecuencia | semanal (cada lunes a las 8 pm) |
| Participantes habituales | equipo Polygon |

## Sprint Review

**Objetivo.** Mostrar al equipo el incremento funcional logrado en el Sprint 1 frente al Sprint Goal planteado.

| Campo | Detalle |
| --- | --- |
| Fecha | 4 de septiembre |
| Participantes | equipo de Polygon y equipo de Sumicolor |

## Retrospectiva

**Objetivo.** Identificar qué funcionó bien, qué no, y qué acciones concretas se llevan al siguiente Sprint para mejorar como equipo.

| Campo | Detalle |
| --- | --- |
| Fecha | 7 de septiembre |
| Participantes | equipo Polygon |
| Qué funcionó bien | se realizó la consolidación de todas la HUs, se actualizó el Backlog del repositorio y se mostraron los bugs encontrados en la página |
| Qué se puede mejorar | Bugs reportados |
| Acciones para el Sprint 2 | El CI/CD pendiente que documentamos en la sección 6.3/6.4 queda pendiente como un acuerdo |

- Evidencia de las ceremonias

<img width="1917" height="867" alt="Captura de pantalla 2026-09-08 231715" src="https://github.com/user-attachments/assets/51a3fe80-b038-4f28-aa9c-ab8db141f583" />

Si se quiere revisar a profundidad cada grabación o acta, diríjase a la pagina de la Wiki titulada "Polygon"

---

# 10. Resultado del Sprint

Al finalizar el Sprint 1, Polygon cuenta con una primera versión demostrable en la que un
usuario autorizado puede ingresar al sistema según su rol y seguir el ciclo completo de una
Orden de Producción desde su creación hasta su entrada al proceso de Mezcla:

- Autenticación, control de acceso por rol y administración de usuarios (creación, edición,
  asignación de roles), con auditoría de cada cambio.
- Creación, edición y consulta de Órdenes de Producción en Planeación.
- Liberación de la Orden de Producción hacia Picky.
- Consulta, registro de materiales preparados y confirmación de envío desde Picky hacia
  Pesaje.
- Consulta, registro del pesaje de materias primas y confirmación de envío desde Pesaje
  hacia Mezcla.

**Aclaración frente a la planeación inicial.** El alcance original de este Sprint (definido
al inicio, sección 3) contemplaba dejar las etapas operativas de Picky, Pesaje, Mezcla y
Extrusión para el Sprint 2. Durante la ejecución, el equipo decidió adelantar y completar
también Picky y Pesaje dentro del Sprint 1 (HU-15 a HU-20), además de las HU de
autenticación, administración y planeación de OP originalmente planeadas. Esto significa que
el resultado obtenido es más amplio que el planeado inicialmente en esas dos etapas.

El resultado de este Sprint constituye la base para que, en el Sprint 2, se implementen las
etapas operativas de **Mezcla, Extrusión, Control de Calidad y Empaque** (y no ya Picky y
Pesaje, adelantadas en este Sprint), junto con las funcionalidades que quedaron pendientes de
esta primera versión: gestión de catálogos (HU-09) y cancelación de una Orden de Producción
(HU-14).

