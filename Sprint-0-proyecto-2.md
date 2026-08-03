#   DEFINICION DEL PRODUCTO

#  SECCION 1.

## Descripción de la empresa

Sumicolor es una empresa reconocida por la fabricación de
**MASTERBATCH®** (concentrados de color) para la industria plástica.
Cuenta con una amplia experiencia en el desarrollo de colores estándar,
especiales y de efectos, trabajando con diferentes polímeros base y
materiales de ingeniería. Entre los materiales que procesa se encuentran
polietileno (PE), polipropileno (PP), poliestireno (PS), PET, EVA,
poliamidas (PA), ABS y POM, entre otros. Sus productos están dirigidos a
diversos procesos de transformación de termoplásticos, como inyección,
soplado, extrusión y rotomoldeo.

## Planteamiento del problema

Actualmente, Sumicolor realiza el seguimiento de sus órdenes de
producción mediante hojas físicas que acompañan cada lote durante todo
el proceso de fabricación. Estas hojas contienen la información y las
actividades que deben ejecutar los diferentes operarios en cada etapa
del proceso productivo.

Al tratarse de documentación en papel, las hojas deben ser trasladadas
manualmente de un operador a otro, lo que dificulta el flujo de
información, aumenta el riesgo de pérdida o deterioro de los documentos
y retrasa la consulta del estado de una orden de producción. Además,
este método representa una carga administrativa adicional, ya que la
información debe revisarse y organizarse manualmente, limitando la
trazabilidad y el control oportuno de los procesos.

## Propuesta de solución

Como solución a esta problemática, se propone el desarrollo de un gestor
de procesos de producción, cuyo objetivo principal será digitalizar las
hojas de seguimiento de las órdenes de producción y centralizar toda la
información en una única plataforma.

El sistema estará diseñado para funcionar tanto en tabletas como en
computadores, permitiendo que cada operario registre el avance de las
actividades correspondientes a su proceso de forma rápida y sencilla.
Asimismo, el software contará con una interfaz intuitiva y fácil de
utilizar, adaptada a las necesidades de los trabajadores de planta,
minimizando la curva de aprendizaje y facilitando su adopción.

Con la implementación de esta solución se espera mejorar la trazabilidad
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

El sistema **Software Polygon** tiene como objetivo digitalizar el
proceso de producción de Sumicolor, reemplazando los formatos físicos
por registros electrónicos realizados desde computadores y tabletas
ubicados en cada una de las bases de producción. Cada orden de
producción (OP) avanza de manera secuencial entre las diferentes etapas
del proceso. El sistema valida que la información requerida en una base
haya sido diligenciada completamente antes de permitir que la orden
continúe hacia la siguiente etapa, garantizando la trazabilidad y
evitando omisiones de información.

## **Administrador del sistema**

El administrador es el usuario encargado de la configuración y
mantenimiento del sistema. Sus principales funciones son:

-   Crear, modificar o desactivar módulos (bases de producción).

-   Agregar, editar o eliminar campos de los formularios de cada módulo
    cuando sea necesario.

-   Administrar usuarios, roles y permisos.

-   Configurar parámetros generales del sistema.

-   Supervisar el correcto funcionamiento de la plataforma.

El administrador no participa directamente en el proceso productivo,
sino que garantiza que el sistema se adapte a las necesidades de la
empresa.

##  **Personal de Planeación**

El proceso inicia en el módulo de Planeación.

El personal de planeación registra la información inicial de la Orden de
Producción, como:

-   Número de orden.

-   Producto a fabricar.

-   Cantidad solicitada.

-   Materia prima requerida.

-   Observaciones.

-   Información necesaria para iniciar la fabricación.

Una vez todos los campos obligatorios estén diligenciados, el sistema
habilita automáticamente el siguiente módulo (Picky).

##  **Personal de Picky**

El personal encargado del área de Picky consulta las órdenes pendientes
y registra la preparación o entrega de las materias primas necesarias
para la producción.

Después de completar la información requerida, el sistema permite que la
orden avance al módulo de Pesaje.

##  **Personal de Pesaje**

El operario de pesaje registra la información correspondiente al pesaje
de las materias primas utilizadas para la orden de producción.

Mientras la información obligatoria no esté completa, el sistema no
permitirá avanzar la orden hacia la siguiente etapa.

Al finalizar correctamente el registro, la orden pasa al módulo de
Mezcla.

##  **Personal de Mezcla**

El operario de mezcla consulta la orden y registra la información
correspondiente al proceso de mezclado de materiales.

Una vez finalizado el registro y validados los datos, el sistema
habilita el módulo de Extrusión.

##  **Personal de Extrusión**

El personal de extrusión registra la información del proceso de
extrusión del material.

El sistema verifica que todos los datos requeridos hayan sido
diligenciados antes de permitir el paso al área de Calidad.

##  **Personal de Calidad**

El área de calidad realiza las inspecciones correspondientes y registra
los resultados obtenidos durante el proceso.

Si la información está completa y cumple con las validaciones
establecidas, la orden continúa hacia el módulo de Empaque.

Por otro lado, si la orden no pasa calidad, se habilitaria un subproceso
donde se determina en que base fallo y dependendiendo el fallo se manda
a tamizaje, saranda, homogenizado, secado o molino

##  **Personal de Empaque**

El operario de empaque registra la información relacionada con el
embalaje y finalización del producto.

Una vez completado el registro, la orden queda lista para ser enviada al
módulo Espacio Cliente.

##  **Espacio Cliente**

En este módulo se registra la información final relacionada con la
entrega del producto al cliente o la finalización del proceso
productivo.

Aquí puede consultarse el historial completo de la orden de producción,
incluyendo todas las etapas por las que pasó y la información registrada
en cada una de ellas.

##  **GLOSARIO DE TERMINOS**

  ----------------------------------------------------------------------------
  **Término**        **Descripción**
  ------------------ ---------------------------------------------------------
  **Administrador    Usuario encargado de configurar el sistema, administrar
  del Sistema**      usuarios, permisos, módulos y realizar modificaciones en
                     los formularios o campos del software.

  **Base de          Cada una de las áreas o estaciones del proceso productivo
  Producción**       donde se ejecuta una actividad específica y se registra
                     información en el sistema.

  **Campo**          Espacio dentro de un formulario destinado a capturar un
                     dato específico, como cantidades, fechas, observaciones o
                     códigos.

  **Computador       Equipo utilizado por personal administrativo o de
  (PC)**             producción para acceder al sistema Software Polygon.

  **Espacio          Módulo final del sistema donde se registra la información
  Cliente**          correspondiente a la finalización de la orden de
                     producción y su disponibilidad para el cliente.

  **Estado de la     Situación actual de una orden de producción dentro del
  Orden**            proceso (Pendiente, En proceso, Finalizada, etc.).

  **Extrusión**      Etapa del proceso productivo donde el material es
                     transformado mediante maquinaria especializada para
                     obtener el producto requerido.

  **Formulario**     Interfaz del sistema utilizada para registrar o consultar
                     información de una orden de producción.

  **Módulo**         Sección funcional del sistema que representa una etapa
                     del proceso de producción, como Planeación, Pesaje o
                     Calidad.

  **Operario**       Usuario encargado de registrar la información
                     correspondiente a la etapa del proceso en la que trabaja.

  **Orden de         Documento que contiene toda la información necesaria para
  Producción (OP)**  fabricar un lote de producto y que recorre todas las
                     etapas del proceso dentro del sistema.

  **PC**             Computador utilizado para acceder a Software Polygon
                     desde las diferentes áreas de la empresa.

  **Pesaje**         Proceso donde se registran las cantidades exactas de
                     materias primas que serán utilizadas en la producción.

  **Planeación**     Primera etapa del proceso productivo donde se crea la
                     orden de producción y se define la información inicial
                     del lote.

  **Picky**          Área encargada de preparar y suministrar las materias
                     primas necesarias para iniciar la producción.

  **Proceso          Conjunto de actividades que transforman las materias
  Productivo**       primas en un producto terminado mediante diferentes
                     etapas.

  **Registro**       Información ingresada por un usuario dentro del sistema
                     durante una etapa del proceso.

  **Rol**            Conjunto de permisos asignados a un usuario para
                     determinar las funciones que puede realizar dentro del
                     sistema.

  **Software         Aplicación web desarrollada para Sumicolor con el
  Polygon**          objetivo de digitalizar y controlar el proceso de
                     producción, reemplazando los registros en papel.

  **SQL Server**     Sistema de gestión de bases de datos donde se almacena
                     toda la información registrada en Software Polygon.

  **Tablet**         Dispositivo móvil utilizado por los operarios para
                     registrar información directamente desde cada base de
                     producción.

  **Trazabilidad**   Capacidad del sistema para registrar y consultar el
                     historial completo de una orden de producción durante
                     todas sus etapas.

  **Usuario**        Persona autorizada para acceder al sistema según el rol
                     asignado.

  **Validación**     Verificación realizada por el sistema para asegurar que
                     los datos ingresados sean correctos y que todos los
                     campos obligatorios hayan sido diligenciados antes de
                     permitir avanzar la orden de producción.
  ----------------------------------------------------------------------------

# Seccion 2.

# Elicitación de Requerimientos

## Objetivo

Identificar las necesidades de la empresa Sumicolor para desarrollar el
software **Polygon**, el cual busca digitalizar el proceso de
producción, reemplazando los formatos físicos por registros digitales
desde computadores y tabletas, mejorando la trazabilidad y el
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

-   No existe trazabilidad en tiempo real.

-   Se requiere un sistema que permita registrar la información desde
    tablets o computadores.

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

 
<img width="707" height="272" alt="image" src="https://github.com/user-attachments/assets/34abe6f3-55f9-413b-be62-a87d0b69b569" />


## Fecha de realización

**Fecha:** Viernes 24 de julio de 2026 2:30 pm*.*

**Lugar:** Instalaciones de Sumicolor.



## Principales requerimientos funcionales identificados

| Código | Requerimiento |
|--------|----------------|
| RF-01 | El sistema debe permitir crear una Orden de Producción. |
| RF-02 | El sistema debe permitir registrar información desde cada base de producción. |
| RF-03 | El sistema debe impedir que una orden avance si la etapa anterior no ha sido completada. |
| RF-04 | El sistema debe permitir consultar el historial completo de una orden. |
| RF-05 | El administrador podrá crear, modificar o eliminar módulos del sistema. |
| RF-06 | El administrador podrá agregar, editar o eliminar campos dentro de cada módulo. |
| RF-07 | El sistema debe administrar usuarios y roles. |
| RF-08 | El sistema debe almacenar toda la información en una base de datos SQL Server. |
| RF-09 | El sistema debe permitir el acceso desde tablets y computadores. |

### EVIDENCIAS

<img width="738" height="1600" alt="WhatsApp Image 2026-08-02 at 2 54 37 PM" src="https://github.com/user-attachments/assets/e7f66efe-1018-4f0a-97d0-8331fafdb1b3" />


### Análisis de la competencia

## Aplicaciones similares

Durante el análisis del mercado se identificaron diferentes soluciones
de tipo **MES (Manufacturing Execution System)** y **ERP con módulos de
manufactura**, las cuales ofrecen funcionalidades similares a las que se
implementarán en **Software Polygon**.

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Aplicación**   **Empresa**                                                      **Costo**                                                   **Diferencia con Software
                                                                                                                                                Polygon**
  ---------------- ---------------------------------------------------------------- ----------------------------------------------------------- ------------------------------
  **Tulip**        [Tulip Interfaces                                                Desde **USD 100 por interfaz/mes** (plan Essentials, mínimo Tulip es una plataforma muy
                   Inc.](https://tulip.co/plans/?utm_source=chatgpt.com)            10 interfaces).                                             completa para la
                                                                                    ([Tulip](https://tulip.co/plans/?utm_source=chatgpt.com))   digitalización industrial,
                                                                                                                                                pero está diseñada para
                                                                                                                                                múltiples industrias y suele
                                                                                                                                                requerir una inversión
                                                                                                                                                considerable. **Software
                                                                                                                                                Polygon** estará desarrollado
                                                                                                                                                específicamente para el
                                                                                                                                                proceso productivo de
                                                                                                                                                Sumicolor, adaptándose
                                                                                                                                                exactamente al flujo de
                                                                                                                                                Planeación → Picky → Pesaje →
                                                                                                                                                Mezcla → Extrusión → Calidad →
                                                                                                                                                Empaque → Espacio Cliente, sin
                                                                                                                                                funcionalidades innecesarias.

  **Odoo           [Odoo                                                            Desde **USD 24,90 por usuario/mes** (plan Estándar). (Odoo) Odoo es un ERP completo que
  Manufacturing    S.A.](https://www.odoo.com/es/pricing?utm_source=chatgpt.com)                                                                incluye contabilidad, ventas,
  (MRP)**                                                                                                                                       inventario, CRM y muchos otros
                                                                                                                                                módulos. La propuesta de
                                                                                                                                                **Software Polygon** se enfoca
                                                                                                                                                exclusivamente en la gestión
                                                                                                                                                de órdenes de producción de
                                                                                                                                                Sumicolor, ofreciendo una
                                                                                                                                                solución más sencilla,
                                                                                                                                                personalizada y fácil de
                                                                                                                                                implementar para la empresa.

  **Prodsmart**    [Prodsmart                                                       Precio bajo cotización; estimaciones del mercado indican    Prodsmart permite controlar la
                   (Autodesk)](https://www.prodsmart.com/?utm_source=chatgpt.com)   aproximadamente **USD 45--165 por usuario/mes**,            producción en tiempo real y
                                                                                    dependiendo del plan y número de usuarios.                  recopilar datos de planta. Sin
                                                                                    (VendorBenchmark)                                           embargo, es una solución
                                                                                                                                                comercial orientada a
                                                                                                                                                diferentes tipos de
                                                                                                                                                fabricantes. **Software
                                                                                                                                                Polygon** incorpora reglas
                                                                                                                                                específicas del proceso de
                                                                                                                                                Sumicolor, como impedir que
                                                                                                                                                una orden avance a la
                                                                                                                                                siguiente base hasta que la
                                                                                                                                                información de la etapa actual
                                                                                                                                                haya sido diligenciada
                                                                                                                                                completamente.
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
