# Offline Handwriting Recognition with Deep Learning and TensorFlow.

Sistema de Deep Learning para el Reconocimiento de Palabras Manuscritas implementado en TensorFlow y entrenado con IAM Handwriting Database.

Sobre este sistema se realiza una validaci�n cruzada y el test IAM.

## Estructura

### Ficheros Python:

- *clean_IAM.py*: Script para limpieza y el preprocesamiento de las im�genes.
- *ANN_model.py*: Modelo de la red neuronal implementada en TensorFlow.
- *cross-validation.py*: Script para la validaci�n cruzada.
- *train.py*: Script para entrenar el modelo y almacenar los par�metros que consiguen un mejor resultado.
- *test.py*: Script para testear un modelo previamente entrenado.
- *hw_utils.py*: Funciones �tiles en distintas partes del proyecto.

### CSV:

- *CSV/Cross-Validation*: Ficheros que contienen los nombres y las transcripciones de las im�genes de los distintos subconjuntos de entrenamiento y validaci�n.
- *CSV/IAM test*: Ficheros que contienen los nombres y las transcripciones de las im�genes para el test IAM.
- *Data/appropriate_images.csv*: Fichero csv que contiene el nombre de las im�genes aptas(87.108).

### Fichero de configuraci�n

Fichero que contiene todos los par�metros del proyecto:

```
"general": Par�metros generales del proyecto.
{
"raw_data_path": Ruta a las im�genes sin preprocesar.
"processed_data_path": Ruta para las im�genes preprocesadas.
"csv_path": Ruta al CSV de im�genes aptas.
"height": Altura de las im�genes preprocesadas.
"width": Anchura de las im�genes preprocesadas.
"dictionary": Diccionario para parsear las etiquetas.
}


"cnn-rnn-ctc": Hiperpar�metros del modelo.
{
"kernel_size": Altura y anchura de los filtros de la CNN, filtros cuadrados.
"num_conv1" :N�mero de neuronas de la 1� capa CNN.
"num_conv2" : N�mero de neuronas de la 2�  capa CNN.
"num_conv3" : N�mero de neuronas de la 3� capa CNN.
"num_conv4" : N�mero de neuronas de la 4� capa CNN.
"num_conv5" : N�mero de neuronas de la 5� capa CNN.
"num_rnn" : N�mero de neuronas de las capas RNNs.
"num_fc" : N�mero de neuronas de la 1� capa Fullconnect.
"num_classes": N�mero de etiquetas, incluida la etiqueta "blanco".
"ctc_input_len": Longitud de la secuencia de entrada a la CTC.
}


"cross-validation": Par�metros para la validaci�n cruzada.
{
"csv_path": Ruta a los CSVs para la validaci�n.
"results_path": Ruta para los resultados.
"num_epochs": N�mero de �pocas.
"validation_period": Periodo de �pocas para realizar la validaci�n del modelo.
"print_period": Periodo de �pocas para la impresi�n por pantalla.
"batch_size": Tama�o del lote de muestras.
}


"IAM-test": Par�metros para el test IAM.
{
"csv_path": Ruta a los CSVs para el test.
"results_path": Ruta para los resultados.
"checkpoints_path": Ruta para almacenar el modelo entrenado.
"num_epochs": N�mero de �pocas.
"validation_period": Periodo de �pocas para realizar la validaci�n del modelo.
"print_period":Periodo de �pocas para la impresi�n por pantalla.
"batch_size" : Tama�o del lote de muestras.
}
```

## Primeros pasos.

### Requisitos Software
Python 3.6 y librer�as:
- TensorFlow 1.3
- PIL
- Pandas
- Numpy
- Json
- Ast


### Instalaci�n y preprocesado de datos.

Tras descargar o clonar el repositorio es necesario descargar el dataset de la [IAM Handwriting Database](http://www.fki.inf.unibe.ch/databases/iam-handwriting-database) y descomprimirlo en el directorio "Offline-Handwriting-Recognition-with-TensorFlow\Data".

Una vez hemos conseguido el dataset ejecutamos:

```
python3 clean_IAM.py [path_config_file]
```
Si no se a�ade ninguna ruta al archivo de configuraci�n se tomar� la ruta por defecto "./config.json"

Este script selecciona las im�genes aptas para las pruebas, las reescala y le a�ade relleno hasta igualar sus dimensiones.

## Ejecuci�n.

### Cross-validation.

Para realizar la validaci�n cruzada del modelo solo es necesario ejecutar:

```
python3 cross-validation.py [path_config_file]
```

Este script realiza 10 validaciones con distintas subdivisiones del dataset original y almacena los resultados en formato CSV.


### Test IAM

El primer paso es entrenar el modelo con el dataset ofrecido por IAM con unas subdivisiones espec�ficas. Para ello ejecutamos:

```
python3 train.py [path_config_file]
```

Este script realiza un entrenamiento del modelo y almacena los par�metros que mejor resultado han dado para el dataset de validaci�n.

Una vez tenemos el modelo entrenado, obtenemos el resultado del test ejecutando:

```
python3 test.py [path_config_file]
```

El resultado se muestra por pantalla y las salidas del sistema se almacenan en CSV.