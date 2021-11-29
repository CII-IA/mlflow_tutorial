# Tutorial MLFflow

En este tutorial revisaremos los fundamentos de MLflow para la admnistración del ciclo de vida de proyectos de Machine Learning.

## Prerequisitos

* Tener listo conda instalado en la computadora de trabajo.
* Tener listo un ambiente alguna librería para realizar tareas de Machine Learning.
* Sobre ese ambiente, instalar MLFlow con el comando: ```conda install -c conda-forge mlflow```

## Sobre MLFlow

MLFlow está organizado en 4 módulos:
* **Tracking**: Es una API y UI que nos permiten registrar los parametros utilizandos en los experimentos, versión del experimento, métricas y archivos de salida.
* **Projects**: Es un estandar para empaquetar el código de proyectos de ciencias de datos y poder reejecutarlo tomando en cuenta sus dependencias.
* **Models**: Es una convención para empaquetar código utilizando diferentes librerías y maneras de desplegarlo. 
* **Model Registry**: Es una API, UI y alamacenamiento de modelos.

Estos módulos pueden ser usados por separado o en conjunto. La filosofía de MLFlow es permitir la mayor flexibilidad en nuestro flujo de trabajo con proyectos con Machine Learning. Por esta razón, se puede usar con una gran variedad de herramientas y librerías de Machine Learning.

Estos 4 módulo de MLFlow se enfoca en atacar los siguientes problemas:
1. Dificultad para mantener un registro adecuado de los experimientos que se realizan. 
2. Dificultad para reproducir código en otras computadoras debido a las dependencias y librerías necesitadas.
3. Falta de estandar para empaquetar y desplegar modelos. 
4. Falta de almacenamiento central para administrar los modelos. 

Es importante mencionar, que existen varias herramientas que por separado nos pueden ayudar a resolver cada uno de los problemas, sim embargo, hay que hacer el esfuerzo de conectarlas entre sí. 

## Ejemplos

### Ejemplo 1: Entrenando y desplegando un regresor lineal

Vamos a revisar un ejemplo completo de uso de MLFlow de principio a fin:
* Entrenaremos un regresor lineal.
* Empaquetaremos el código que entrena el modelo en una forma reutilizable.
* Desplegaremos el modelo como un servidor simple HTTP que se podrá usar para inferir predicciones.

