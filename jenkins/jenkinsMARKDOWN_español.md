# Resumen de Jenkins

## Introducción
Jenkins es una herramienta de automatización de código abierto, probablemente la más utilizada en el mundo. Está escrita en Java y se ejecuta en la JVM. Su gran ventaja es su extensibilidad gracias a un ecosistema de plugins que permiten ampliar su funcionalidad. Se pueden desarrollar plugins en Java, pero ya existen muchos creados por la comunidad.

## Escalabilidad
Jenkins permite escalar tanto horizontal como verticalmente. Puede ejecutar múltiples trabajos de forma concurrente en una sola máquina. Si los recursos no son suficientes, se pueden agregar más. Además, si una sola máquina no es suficiente, se puede escalar horizontalmente utilizando "slaves" (nodos de trabajo) para distribuir las tareas.

## Seguridad y Actualizaciones
Jenkins recibe actualizaciones constantes, incluyendo mejoras de seguridad, ya que suele ser un objetivo importante dentro de la infraestructura de una empresa debido a la cantidad de datos y procesos que maneja.

## Pipelines as Code
Una de las mejoras más importantes en los últimos años es la capacidad de definir "Jobs" mediante código. Esto permite que la automatización sea programática y facilita la migración de configuraciones a otros servidores Jenkins de manera reproducible. Esta funcionalidad es conocida como **Pipelines as Code**.

## Jobs y Build Executor
La parte más importante de Jenkins son los **Jobs**, que representan los trabajos que ejecuta. Jenkins puede ejecutar varios Jobs simultáneamente gracias al **Build Executor**. 

- Cada Job tiene su propio espacio de trabajo dentro del directorio `/var/lib/jenkins/workspace/`.
- Una **ejecución** de un Job se conoce como **Build**.
- Cada Job tiene un **Historial de Builds** para auditoría y seguimiento de ejecuciones.

Existen diferentes tipos de Jobs, como:
- **Freestyle Project**
- **Pipeline**
- **Multi-configuration Project**
- **Folder**, entre otros.

Cada ejecución de un Job se agrega al **Build History**, lo que permite rastrear cuál fue el último éxito o fallo.

## Trabajando con Jobs Encadenados
Para encadenar Jobs en Jenkins, se pueden utilizar plugins como **Parameterized Trigger**. Ejemplo de configuración:

1. Instalar el plugin **Parameterized Trigger** y reiniciar Jenkins.
2. Crear dos Jobs:
   - **watchers**: Se configura con la opción "Build after other projects are built" para ejecutarse después de que otro Job (ejemplo: `hello-platzi`) haya finalizado exitosamente.
   - **parameterized**: Acepta parámetros de entrada. Se habilita la opción "This project is parameterized" y se define una variable `ROOT_ID`.
3. En `hello-platzi`, se configura la ejecución de `parameterized` utilizando "Trigger/call build on other projects" con parámetros predefinidos: `ROOT_ID=$BUILD_NUMBER`.
4. Al ejecutar `hello-platzi`, este llama a `parameterized`, que a su vez puede activar `watchers`.

El uso de **Parameterized Jobs** es más flexible que **watchers**, ya que permite definir más opciones de ejecución y encadenamiento de trabajos.

## Integración con GitHub
Es posible conectar Jenkins con un repositorio en GitHub para ejecutar un build automáticamente después de cada `push`. Para esto, se deben hacer configuraciones en ambos sistemas:

### Configuración en Jenkins
1. Instalar el plugin **GitHub**.
2. Crear un Job y seleccionar la opción **Git** en la sección SCM.
3. Ingresar la URL del repositorio.
4. En "Branches to build", dejar en blanco para aceptar cualquier rama.
5. En "Build Triggers", seleccionar "GitHub hook trigger for GITScm polling".

### Configuración en GitHub
1. Ir a **Settings** → **Webhooks** en el repositorio.
2. Añadir un nuevo Webhook.
3. Configurar la **Payload URL** con la dirección de Jenkins (`/github-webhook/`).
4. Seleccionar la opción **Just the push event**.

## Pipelines en Jenkins
Los Pipelines permiten definir Jobs mediante código en lugar de configurarlos manualmente en la interfaz visual. Existen dos tipos:
- **Declarativo**
- **Scripted**

### Conceptos Claves en Pipelines
- **Pipeline**: Representa un modelo de entrega continua definido en código.
- **Node**: Máquina en la que Jenkins ejecuta el Pipeline.
- **Stage**: Bloque que define una fase en el proceso (ejemplo: "Build", "Test", "Deploy").
- **Step**: Tarea específica dentro de un Stage (por ejemplo, ejecutar `sh 'make'`).

Se puede generar código automáticamente desde "Pipeline Syntax" en Jenkins para obtener fragmentos de código de pasos específicos.

## Distribución de Carga con Slaves
Jenkins permite distribuir la ejecución de Jobs en múltiples máquinas conocidas como **Slaves**. Estas se conectan al nodo maestro y reciben trabajos de este. Pueden ser máquinas físicas o virtuales y permiten escalar horizontalmente la infraestructura de Jenkins.

---
Este resumen cubre los aspectos clave de Jenkins, su escalabilidad, seguridad, integración con GitHub y el uso de Pipelines para una automatización eficiente.

