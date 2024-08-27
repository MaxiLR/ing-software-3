## Trabajo Práctico 4 - Azure Devops Pipelines

### Consignas a desarrollar en el trabajo práctico:

**Azure DevOps Pipelines**

- Breve Descripción de Azure DevOps Pipelines

  _**Azure DevOps Pipelines** es un servicio dentro de Azure DevOps que facilita la integración y entrega continua (CI/CD) de aplicaciones. Permite a los equipos de desarrollo automatizar las fases de compilación, prueba y despliegue de sus aplicaciones, garantizando que los cambios en el código se integren y se entreguen de manera eficiente y confiable._

- Tipos de Pipelines: Build y Deploy

  _1. **Build Pipelines**: Se enfocan en la integración continua (CI). En esta etapa, el código es compilado, se ejecutan pruebas automatizadas, y se generan artefactos (por ejemplo, archivos ejecutables, paquetes de contenedores, etc.). El objetivo principal es asegurar que cada commit en el repositorio funcione correctamente y esté listo para ser desplegado._

  _2. **Deploy Pipelines**: Se encargan de la entrega continua (CD). Aquí es donde los artefactos generados en el Build Pipeline se despliegan en entornos específicos, como desarrollo, pruebas, producción, etc. Este proceso puede incluir pasos como la configuración de infraestructura, despliegue de aplicaciones, y ejecución de scripts de post-despliegue._

- Diferencias entre Editor Clásico y YAML

  _- **Editor Clásico**: Ofrece una interfaz gráfica para crear y configurar los pipelines. Es más intuitivo para usuarios nuevos, ya que permite arrastrar y soltar tareas y configurar sus propiedades a través de formularios. Es ideal para quienes prefieren trabajar con interfaces gráficas en lugar de código._

  _- **YAML**: Utiliza un archivo de texto (`azure-pipelines.yml`) para definir el pipeline. Este enfoque basado en código ofrece mayor flexibilidad, versionado junto con el código fuente, y es más fácil de compartir y mantener en grandes equipos. Aunque tiene una curva de aprendizaje más pronunciada, se considera más potente y escalable para usuarios avanzados._

- Agentes: Microsoft-Hosted y Self-Hosted

  _- **Microsoft-Hosted Agents**: Son agentes de compilación y despliegue proporcionados por Azure. Son gestionados y mantenidos por Microsoft, lo que significa que siempre están actualizados con las últimas herramientas y versiones de software. Son ideales para la mayoría de los proyectos, ya que no requieren configuración adicional por parte del usuario._

  _- **Self-Hosted Agents**: Son agentes que los usuarios configuran y gestionan en sus propios servidores. Ofrecen mayor control sobre el entorno de compilación y despliegue, lo que es útil para proyectos que requieren software específico, versiones no soportadas en los agentes de Microsoft, o para casos en que se necesita mayor capacidad de procesamiento o almacenamiento local. Sin embargo, requieren más mantenimiento y configuración por parte del equipo._

#### 4- Pasos del TP

- 4.1 Verificar acceso a Pipelines concedido
  ![alt text](image.png)
- 4.2 Agregar en pipeline YAML una tarea de Publish.
  ![alt text](image-1.png)
  ![alt text](image-2.png)
- 4.3 Explicar por qué es necesario contar con una tarea de Publish en un pipeline que corre en un agente de Microsoft en la nube.
  _La tarea de Publish en un pipeline que corre en un agente de Microsoft en la nube es esencial para guardar y compartir los artefactos generados (como ejecutables o paquetes) porque los agentes en la nube no mantienen datos entre ejecuciones. Sin esta tarea, los artefactos podrían perderse al terminar el pipeline._
- 4.4 Descargar el resultado del pipeline y correr localmente el software compilado.
  ![alt text](image-3.png)
  ![alt text](image-4.png)
  FALTA EJECUCION
- 4.5 Habilitar el editor clásico de pipelines. Explicar las diferencias claves entre este tipo de editor y el editor YAML.
  ![alt text](image-5.png)
  _El Classic Editor es mas para principiantes y el YAML esta orientado a usuarios mas tecnicos y experimentados._
- 4.6 Crear un nuevo pipeline con el editor clásico. Descargar el resultado del pipeline y correr localmente el software compilado.
  ![alt text](image-6.png)
  ![alt text](image-7.png)
  ![alt text](image-8.png)
  ![alt text](image-9.png)
  ![alt text](image-10.png)
  ![alt text](image-11.png)
  FALTA EJECUCION
- 4.7 Configurar CI en ambos pipelines (YAML y Classic Editor). Mostrar resultados de la ejecución automática de ambos pipelines al hacer un commit en la rama main.
  ![alt text](image-18.png)
  ![alt text](image-19.png)
  ![alt text](image-20.png)
  ![alt text](image-21.png)
- 4.8 Explicar la diferencia entre un agente MS y un agente Self-Hosted. Qué ventajas y desventajas hay entre ambos? Cuándo es conveniente y/o necesario usar un Self-Hosted Agent?
  _- **Microsoft-Hosted Agents**: Son agentes de compilación y despliegue proporcionados por Azure. Son gestionados y mantenidos por Microsoft, lo que significa que siempre están actualizados con las últimas herramientas y versiones de software. Son ideales para la mayoría de los proyectos, ya que no requieren configuración adicional por parte del usuario._
  _- **Self-Hosted Agents**: Son agentes que los usuarios configuran y gestionan en sus propios servidores. Ofrecen mayor control sobre el entorno de compilación y despliegue, lo que es útil para proyectos que requieren software específico, versiones no soportadas en los agentes de Microsoft, o para casos en que se necesita mayor capacidad de procesamiento o almacenamiento local. Sin embargo, requieren más mantenimiento y configuración por parte del equipo._

- 4.8 Crear un Pool de Agentes y un Agente Self-Hosted
  ![alt text](image-12.png)
  ![alt text](image-13.png)
  ![alt text](image-14.png)
- 4.9 Instalar y correr un agente en nuestra máquina local.
  ![alt text](image-15.png)
  ![alt text](image-16.png)
  ![alt text](image-17.png)
- 4.10 Crear un pipeline que use el agente Self-Hosted alojado en nuestra máquina local.
  ![alt text](image-22.png)
  ![alt text](image-23.png)
- 4.11 Buscar el resultado del pipeline y correr localmente el software compilado.
  ![alt text](image-24.png)
  FALTA EJECUCION
- 4.12 Crear un nuevo proyecto en ADO clonado desde un repo que contenga una aplicación en Angular como por ejemplo https://github.com/ingsoft3ucc/angular-demo-project.git
  ![alt text](image-25.png)
  ![alt text](image-26.png)
  ![alt text](image-27.png)
- 4.13 Configurar un pipeline de build para un proyecto de tipo Angular como el clonado.
  ![alt text](image-28.png)
  ![alt text](image-29.png)
  ![alt text](image-30.png)
- 4.14 Habilitar CI para el pipeline.
  ![alt text](image-31.png)
- 4.15 Hacer un cambio a un archivo del proyecto (algún cambio en el HTML que se renderiza por ejemplo) y verificar que se ejecute automáticamente el pipeline.
  ![alt text](image-32.png)
  ![alt text](image-33.png)
  ![alt text](image-34.png)
  ![alt text](image-35.png)
- 4.16 Descargar el resultado del pipeline y correr en un servidor web local el sitio construido.
- 4.17 Mostrar el antes y el después del cambio.

#### 5- Criterio de Calificación

Los pasos 4.1 al 4.11 representan un 60% de la nota total, los pasos 4.12 al 4.17 representan el 40% restante
