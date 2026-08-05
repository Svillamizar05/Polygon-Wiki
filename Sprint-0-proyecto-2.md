#   DEFINICION DEL PRODUCTO

#  SECCION 1.

## Descripción de la empresa

Sumicolor es una empresa reconocida por la fabricación de [MASTERBATCH](#glosario-api-m) (concentrados de color) para la industria plástica. Cuenta con una amplia experiencia en el desarrollo de colores estándar, especiales y de efectos, trabajando con diferentes polímeros base y materiales de ingeniería. Entre los materiales que procesa se encuentran polietileno (PE), polipropileno (PP), poliestireno (PS), PET, EVA, poliamidas (PA), ABS y POM, entre otros. Sus productos están dirigidos a diversos procesos de transformación de termoplásticos, como inyección, soplado, [extrusión](#glosario-api) y rotomoldeo.

## Planteamiento del problema

Actualmente, Sumicolor realiza el seguimiento de sus órdenes de
producción mediante hojas físicas que acompañan cada lote durante todo
el proceso de fabricación. Estas hojas contienen la información y las
actividades que deben ejecutar los diferentes [operarios](#glosario-api) en cada etapa
del [proceso productivo](#glosario-api).

Al tratarse de documentación en papel, las hojas deben ser trasladadas
manualmente de un operador a otro, lo que dificulta el flujo de
información, aumenta el riesgo de pérdida o deterioro de los documentos
y retrasa la consulta del estado de una [orden de producción](#glosario-api). Además,
este método representa una carga administrativa adicional, ya que la
información debe revisarse y organizarse manualmente, limitando la
[trazabilidad](#glosario-api-t) y el control oportuno de los procesos.

## Propuesta de solución

Como solución a esta problemática, se propone el desarrollo de un gestor
de procesos de producción, cuyo objetivo principal será digitalizar las
hojas de seguimiento de las órdenes de producción y centralizar toda la
información en una única plataforma.

El sistema estará diseñado para funcionar tanto en [tabletas](#glosario-api-ta) como en
[computadores](#glosario-api), permitiendo que cada [operario](#glosario-api) registre el avance de las
actividades correspondientes a su proceso de forma rápida y sencilla.
Asimismo, el software contará con una interfaz intuitiva y fácil de
utilizar, adaptada a las necesidades de los trabajadores de planta,
minimizando la curva de aprendizaje y facilitando su adopción.

Con la implementación de esta solución se espera mejorar la [trazabilidad](#glosario-api-t)
de las órdenes de producción, optimizar el flujo de información entre
las diferentes áreas, reducir el uso de papel, disminuir los errores
asociados al manejo manual de documentos y proporcionar a la
administración un seguimiento en tiempo real del estado de cada proceso
productivo.

## **Personas y roles del proyecto**

  
<img width="360" height="555" alt="image" src="https://github.com/user-attachments/assets/1e5ef3f8-e0ef-4d1b-9b8c-b7113fbe226c" />


>## **Público objetivo y contexto**

<img width="1132" height="352" alt="DcontextoP2 drawio" src="https://github.com/user-attachments/assets/aa88f43e-2824-4a32-a1b1-6dc531f3101a" />


## Descripción textual de la interacción de los usuarios con el sistema

El sistema [Software Polygon](#glosario-api-sp) tiene como objetivo digitalizar el
proceso de producción de Sumicolor, reemplazando los formatos físicos
por [registros](#glosario-api) electrónicos realizados desde [computadores](#glosario-api) y [tabletas](#glosario-api-ta)
ubicados en cada una de las bases de producción. Cada orden de
producción (OP) avanza de manera secuencial entre las diferentes etapas
del proceso. El sistema valida que la información requerida en una base
haya sido diligenciada completamente antes de permitir que la orden
continúe hacia la siguiente etapa, garantizando la [trazabilidad](#glosario-api-t) y
evitando omisiones de información.

## **Administrador del sistema**

El administrador es el [usuario](#glosario-api) encargado de la configuración y
mantenimiento del sistema. Sus principales funciones son:

-   Crear, modificar o desactivar [módulos](#glosario-api) (bases de producción).

-   Agregar, editar o eliminar [campos](#glosario-api) de los [formularios](#glosario-api) de cada [módulo](#glosario-api)
    cuando sea necesario.

-   Administrar [usuarios](#glosario-api), [roles](#glosario-api) y permisos.

-   Configurar parámetros generales del sistema.

-   Supervisar el correcto funcionamiento de la plataforma.

El administrador no participa directamente en el [proceso productivo](#glosario-api),
sino que garantiza que el sistema se adapte a las necesidades de la
empresa.

##  **Personal de Planeación**

El proceso inicia en el [módulo](#glosario-api) de [Planeación](#glosario-api).

El personal de [planeación](#glosario-api) registra la información inicial de la Orden de
Producción, como:

-   Número de orden.

-   Producto a fabricar.

-   Cantidad solicitada.

-   Materia prima requerida.

-   Observaciones.

-   Información necesaria para iniciar la fabricación.

Una vez todos los [campos](#glosario-api) obligatorios estén diligenciados, el sistema
habilita automáticamente el siguiente [módulo](#glosario-api) ([Picky](#glosario-api)).

##  **Personal de Picky**

El personal encargado del área de [Picky](#glosario-api) consulta las órdenes pendientes
y registra la preparación o entrega de las materias primas necesarias
para la producción.

Después de completar la información requerida, el sistema permite que la
orden avance al [módulo](#glosario-api) de [Pesaje](#glosario-api).

##  **Personal de Pesaje**

El [operario](#glosario-api) de [pesaje](#glosario-api) registra la información correspondiente al [pesaje](#glosario-api)
de las materias primas utilizadas para la [orden de producción](#glosario-api).

Mientras la información obligatoria no esté completa, el sistema no
permitirá avanzar la orden hacia la siguiente etapa.

Al finalizar correctamente el [registro](#glosario-api), la orden pasa al [módulo](#glosario-api) de
Mezcla.

##  **Personal de Mezcla**

El [operario](#glosario-api) de mezcla consulta la orden y registra la información
correspondiente al proceso de mezclado de materiales.

Una vez finalizado el [registro](#glosario-api) y validados los datos, el sistema
habilita el [módulo](#glosario-api) de [extrusión](#glosario-api).

##  **Personal de Extrusión**

El personal de [extrusión](#glosario-api) registra la información del proceso de
[extrusión](#glosario-api) del material.

El sistema verifica que todos los datos requeridos hayan sido
diligenciados antes de permitir el paso al área de Calidad.

##  **Personal de Calidad**

El área de calidad realiza las inspecciones correspondientes y registra
los resultados obtenidos durante el proceso.

Si la información está completa y cumple con las validaciones
establecidas, la orden continúa hacia el [módulo](#glosario-api) de Empaque.

Por otro lado, si la orden no pasa calidad, se habilitaria un subproceso
donde se determina en que base fallo y dependendiendo el fallo se manda
a tamizaje, saranda, homogenizado, secado o molino

##  **Personal de Empaque**

El [operario](#glosario-api) de empaque registra la información relacionada con el
embalaje y finalización del producto.

Una vez completado el [registro](#glosario-api), la orden queda lista para ser enviada al
[módulo](#glosario-api) [Espacio Cliente](#glosario-api).

##  **Espacio Cliente**

En este [módulo](#glosario-api) se registra la información final relacionada con la
entrega del producto al cliente o la finalización del proceso
productivo.

Aquí puede consultarse el historial completo de la [orden de producción](#glosario-api),
incluyendo todas las etapas por las que pasó y la información registrada
en cada una de ellas.

##  GLOSARIO DE TERMINOS


| Término | Descripción |
|---------|--------------|
| <a id="glosario-api"></a>**Administrador del Sistema** | Usuario encargado de configurar el sistema, administrar usuarios, permisos, módulos y realizar modificaciones en los formularios o campos del software. |
| <a id="glosario-api"></a>**Base de Producción** | Cada una de las áreas o estaciones del proceso productivo donde se ejecuta una actividad específica y se registra información en el sistema. |
| <a id="glosario-api"></a>**Campo** | Espacio dentro de un formulario destinado a capturar un dato específico, como cantidades, fechas, observaciones o códigos. |
| <a id="glosario-api"></a>**Computador (PC)** | Equipo utilizado por personal administrativo o de producción para acceder al sistema Software Polygon. |
| <a id="glosario-api"></a>**Espacio Cliente** | Módulo final del sistema donde se registra la información correspondiente a la finalización de la orden de producción y su disponibilidad para el cliente. |
| <a id="glosario-api"></a>**Estado de la Orden** | Situación actual de una orden de producción dentro del proceso (Pendiente, En proceso, Finalizada, etc.). |
| <a id="glosario-api"></a>**Extrusión** | Etapa del proceso productivo donde el material es transformado mediante maquinaria especializada para obtener el producto requerido. |
| <a id="glosario-api"></a>**Formulario** | Interfaz del sistema utilizada para registrar o consultar información de una orden de producción. |
| <a id="glosario-api"></a>**Módulo** | Sección funcional del sistema que representa una etapa del proceso de producción, como Planeación, Pesaje o Calidad. |
| <a id="glosario-api"></a>**Operario** | Usuario encargado de registrar la información correspondiente a la etapa del proceso en la que trabaja. |
| <a id="glosario-api"></a>**Orden de Producción (OP)** | Documento que contiene toda la información necesaria para fabricar un lote de producto y que recorre todas las etapas del proceso dentro del sistema. |
| <a id="glosario-api"></a>**PC** | Computador utilizado para acceder a Software Polygon desde las diferentes áreas de la empresa. |
| <a id="glosario-api"></a>**Pesaje** | Proceso donde se registran las cantidades exactas de materias primas que serán utilizadas en la producción. |
| <a id="glosario-api"></a>**Planeación** | Primera etapa del proceso productivo donde se crea la orden de producción y se define la información inicial del lote. |
| <a id="glosario-api"></a>**Picky** | Área encargada de preparar y suministrar las materias primas necesarias para iniciar la producción. |
| <a id="glosario-api"></a>**Proceso Productivo** | Conjunto de actividades que transforman las materias primas en un producto terminado mediante diferentes etapas. |
| <a id="glosario-api"></a>**Registro** | Información ingresada por un usuario dentro del sistema durante una etapa del proceso. |
| <a id="glosario-api"></a>**Rol** | Conjunto de permisos asignados a un usuario para determinar las funciones que puede realizar dentro del sistema. |
| <a id="glosario-api-sp"></a>**Software Polygon** | Aplicación web desarrollada para Sumicolor con el objetivo de digitalizar y controlar el proceso de producción, reemplazando los registros en papel. |
| <a id="glosario-api-sql"></a>**SQL Server** | Sistema de gestión de bases de datos donde se almacena toda la información registrada en Software Polygon. |
| <a id="glosario-api-ta"></a>**Tablet** | Dispositivo móvil utilizado por los operarios para registrar información directamente desde cada base de producción. |
| <a id="glosario-api-t"></a>**Trazabilidad** | Capacidad del sistema para registrar y consultar el historial completo de una orden de producción durante todas sus etapas. |
| <a id="glosario-api"></a>**Usuario** | Persona autorizada para acceder al sistema según el rol asignado. |
| <a id="glosario-api-m"></a>**MASTERBATCH** | Es una mezcla concentrada de pigmentos, colorantes o aditivos funcionales dispersos en una resina polimérica portadora (carrier), presentada en forma de gránulos sólidos. |

# Seccion 2.

# Elicitación de Requerimientos

## Objetivo

Identificar las necesidades de la empresa Sumicolor para desarrollar el
software **Polygon**, el cual busca digitalizar el proceso de
producción, reemplazando los formatos físicos por [registros](#glosario-api) digitales
desde [computadores](#glosario-api) y [tabletas](#glosario-api-ta), mejorando la [trazabilidad](#glosario-api-t) y el
seguimiento de las órdenes de producción.

# Técnicas utilizadas

## 1. Entrevista semiestructurada

**Descripción**

Se realizó una entrevista con el personal de la empresa para conocer el
proceso actual de producción, identificar los problemas existentes y
comprender las necesidades que debe satisfacer el sistema.

**Objetivo**

Obtener información directamente de los responsables del proceso
productivo.

**Información obtenida**

-   Actualmente la información se registra en hojas físicas.

-   Las órdenes de producción pasan manualmente entre las diferentes
    bases.

-   Se presentan dificultades para consultar información histórica.

-   No existe [trazabilidad](#glosario-api-t) en tiempo real.

-   Se requiere un sistema que permita registrar la información desde
    [tabletas](#glosario-api-ta) o [computadores](#glosario-api).

-   Cada orden debe avanzar únicamente cuando la etapa anterior haya
    sido completada.

## 2. Revisión documental

**Descripción**

Se revisaron las órdenes de producción utilizadas actualmente por la
empresa para identificar la información registrada en cada etapa del
proceso.

**Objetivo**

Determinar qué datos deberán almacenarse dentro del sistema.

**Información obtenida**

Se identificaron:

-   Número de orden

-   Información del lote

-   Materias primas

-   Cantidades

-   Operario responsable

-   Fechas

-   Observaciones

-   Resultados de calidad

 
| Participante | Rol |
|---|---|
| Equipo de Proyecto 2 | Analistas de requerimientos y encargados de recopilar la información necesaria para el desarrollo del sistema. |
| Jefe de Producción de Sumicolor | Explicó el flujo del [proceso productivo](#glosario-api), las necesidades de la empresa y los problemas del proceso actual basado en formatos físicos. |
| Ingeniero de Procesos de Sumicolor | Describió las actividades realizadas en cada [base de producción](#glosario-api), los datos que deben registrarse y los requerimientos funcionales del sistema. |
| Equipo de TI de Sumicolor | Explicó la infraestructura tecnológica actual de la empresa, los sistemas existentes (como [SQL Servers](#glosario-api-sql), Visual Control y Excel), y brindó orientación sobre la viabilidad técnica e integración del [Software Polygon](#glosario-api-sp) con los recursos tecnológicos disponibles. |


## Fecha de realización

**Fecha:** Viernes 24 de julio de 2026 2:30 pm*.*

**Lugar:** Instalaciones de Sumicolor.



## Principales requerimientos funcionales identificados

| Código | Requerimiento |
|--------|----------------|
| RF-01 | El sistema debe permitir crear una [Orden de Producción](#glosario-api). |
| RF-02 | El sistema debe permitir registrar información desde cada [base de producción](#glosario-api). |
| RF-03 | El sistema debe impedir que una orden avance si la etapa anterior no ha sido completada. |
| RF-04 | El sistema debe permitir consultar el historial completo de una orden. |
| RF-05 | El administrador podrá crear, modificar o eliminar [módulos](#glosario-api) del sistema. |
| RF-06 | El administrador podrá agregar, editar o eliminar [campos](#glosario-api) dentro de cada [módulo](#glosario-api). |
| RF-07 | El sistema debe administrar [usuarios](#glosario-api) y roles. |
| RF-08 | El sistema debe almacenar toda la información en una base de datos [SQL Servers](#glosario-api-sql). |
| RF-09 | El sistema debe permitir el acceso desde [tabletas](#glosario-api-ta) y [computadores](#glosario-api). |

### EVIDENCIAS

<img width="738" height="1600" alt="WhatsApp Image 2026-08-02 at 2 54 37 PM" src="https://github.com/user-attachments/assets/e7f66efe-1018-4f0a-97d0-8331fafdb1b3" />


### Análisis de la competencia

## Aplicaciones similares

Durante el análisis del mercado se identificaron diferentes soluciones
de tipo **MES (Manufacturing Execution System)** y **ERP con [módulos](#glosario-api) de
manufactura**, las cuales ofrecen funcionalidades similares a las que se
implementarán en [Software Polygon](#glosario-api-sp).

 | Aplicación | Empresa | Costo | Diferencia con Software Polygon |
|---|---|---|---|
| **Tulip** | [Tulip Interfaces Inc.](https://tulip.co/plans/?utm_source=chatgpt.com) | Desde **USD 100 por interfaz/mes** (plan Essentials, mínimo 10 interfaces). ([Tulip](https://tulip.co/plans/?utm_source=chatgpt.com)) | Tulip es una plataforma muy completa para la digitalización industrial, pero está diseñada para múltiples industrias y suele requerir una inversión considerable. [Software Polygon](#glosario-api-sp) estará desarrollado específicamente para el [proceso productivo](#glosario-api) de Sumicolor, adaptándose exactamente al flujo de [Planeación](#glosario-api) → [Picky](#glosario-api) → [Pesaje](#glosario-api) → Mezcla → [Extrusión](#glosario-api) → Calidad → Empaque → [Espacio Cliente](#glosario-api), sin funcionalidades innecesarias. |
| **Odoo Manufacturing (MRP)** | [Odoo S.A.](https://www.odoo.com/es/pricing?utm_source=chatgpt.com) | Desde **USD 24,90 por [usuario](#glosario-api)/mes** (plan Estándar). (Odoo) | Odoo es un ERP completo que incluye contabilidad, ventas, inventario, CRM y muchos otros [módulos](#glosario-api). La propuesta de [Software Polygon](#glosario-api-sp) se enfoca exclusivamente en la gestión de órdenes de producción de Sumicolor, ofreciendo una solución más sencilla, personalizada y fácil de implementar para la empresa. |
| **Prodsmart** | [Prodsmart (Autodesk)](https://www.prodsmart.com/?utm_source=chatgpt.com) | Precio bajo cotización; estimaciones del mercado indican aproximadamente **USD 45–165 por [usuario](#glosario-api)/mes**, dependiendo del plan y número de [usuarios](#glosario-api). (VendorBenchmark) | Prodsmart permite controlar la producción en tiempo real y recopilar datos de planta. Sin embargo, es una solución comercial orientada a diferentes tipos de fabricantes. [Software Polygon](#glosario-api-sp) incorpora reglas específicas del proceso de Sumicolor, como impedir que una orden avance a la siguiente base hasta que la información de la etapa actual haya sido diligenciada completamente. |

# Seccion 3.
## Story Mapping
https://miro.com/welcomeonboard/cXViUWdFWHlOY3l4VUtpbXg4bzhYa1ZXNlJMSnJQV2w2aGpjaDgvZFF2bS9acjFnTFRhQ1RxajdrZXdzWEpiRWcxQms1M1M2ZUpWR1NyQlNmelRvZ2pnS0lLTUllUWdqOWdMamQrMlpSRUxaMS95Tk9BZ0UyZ2dPSUlyMDBiVlVQdGo1ZEV3bUdPQWRZUHQzSGl6V2NBPT0hdjE=?share_link_id=363086209415

## Backlog
https://github.com/users/SimonCastroV/projects/10

## Sprint 1 Planning 
<img width="1505" height="688" alt="image" src="https://github.com/user-attachments/assets/0342f992-c01e-4089-b5d3-00c955e0de5d" />


