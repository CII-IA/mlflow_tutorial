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
* Entrenaremos un regresor lineal (```python ejemplo1_elasticnet_wine\train.py```).
* Podemos revisar el desempeño del modelo ejecutando ``mlflow ui``. 
* Empaquetaremos el código que entrena el modelo en una forma reutilizable aprovechando el módulo de Projects de MLFlow (archivo: MLproject).
* Desplegaremos el modelo como un servidor simple HTTP que se podrá usar para inferir predicciones (``mlflow models serve -m <enlace al modelo como aparece en mlflowui> -p <puerto>``). Esto nos creará todo el ambiente necesario para utilizarlo a través de un servidor HTTP sencillo.
* Mandaremos datos para ejecutar una predicción:
- ``curl.exe -X POST -H "Content-Type:application/json" --data '{"columns":["alcohol", "chlorides", "citric acid", "density", "fixed acidity", "free sulfur dioxide", "pH", "residual sugar", "sulphates", "total sulfur dioxide", "volatile acidity"],"data":[[12.8, 0.029, 0.48, 0.98, 6.2, 29, 3.33, 1.2, 0.39, 75, 0.66]]}' http://127.0.0.1:1234/invocations``
* Podemos volver a activar el servidor con: ``conda activate mlflow-ffcb2c5031964a28ac387f0ae92502be7ea9a383 & waitress-serve --host=127.0.0.1 --port=1234 --ident=mlflow mlflow.pyfunc.scoring_server.wsgi:app``

### Ejemplo 2: Optimización de hiperparámetros 

Una tarea que puede consumir mucho tiempo es la selección correcta de hiperparámetros. Con MLFlow es posible realizar tareas de optimización de los hiperparámetros. En este projecto se incluyen 4 módulos:

* train: Se enfoca en entrenar el modelo base dee Keras con hyperparámetros provistos en la línea de comandos.
* search_random: Hace un muestreo de los hyperparámetros extraídos de una distribución uniforme y corre diferentes experimentos con ellos.
* search_gpyopt: Es igual que search_random pero utiliza optimización basada en Gaussian Process.
* search_hyperopt: Utiliza el método TPE (Trees of Parzen Estimators) para realizar la optimización de hyperparámetros.

Debido a que vamos a ejecutar varios métodos de optimización debemos de organizarlos. Esta organización los experimentos de alguna de las siguientes formas (hay que ejecutar las instrucciones desde la carpeta raíz de todos los ejemplos):
* Podemos crear un __experimento__, bajo el cuál se guardarán todas las ejecuciones asociadas a él.
1. ``mlflow experiments create -n individual_runs``. Esto crea un experimentos y nos regresa un ID que vamos a utilizar después.
2. ``mlflow run -e <entry point from MLproject> --experiment-id <individual_runs_experiment_id> ejemplo2_opt_hyper``. Esto nos permite correr un entry point del proyecto sobre el experimento definido anteriormente.
