# Laboratorio 2 - Regresión

[Objetivos](#objetivos)

[Herramientas](#herramientas)

[Caso de negocio](#enunciado)

[Entregables](#entregables)

[Rúbrica de clasificación](#rubrica)


## <a name="objetivos"></a> Objetivos

- Construir modelos analíticos para estimar una variable objetivo continua a partir de un conjunto de variables observadas.

- Comprender el proceso para la construcción de modelos analíticos que responden a una tarea de regresión.

- Automatizar el proceso de construcción de modelos analíticos con el uso de pipelines de tal forma que puedan ser usados en ambientes de producción.

- Extraer información útil para el negocio a partir de los resultados de los modelos de regresión.


## <a name="herramientas"></a> Herramientas

Durante este laboratorio se trabajará con las siguientes herramientas:

- Librerías de Python para el procesamiento y analisis de datos como: 
    - Pandas
    - Scikit-Learn
    - Matplotlib, Seaborn
- Entorno de desarrollo Jupyter Lab instalado localmente mediante Anaconda o en la nube mediante Google Colab.
- Software para la construcción de dashboards como Power BI o Tableau.

## <a name="enunciado"></a> Caso de Negocio: Predicción de Precios de portátiles para CompuAlpes

CompuAlpes es una reconocida tienda minorista que vende computadores portátiles de diferentes fabricantes y especificaciones técnicas. Con el auge de la tecnología y el creciente número de productos en el mercado, la empresa busca optimizar sus estrategias de fijación de precios y promociones para seguir siendo competitiva. Es en este último punto, donde ha identificado un reto relacionado con determinar el precio adecuado para un portatil ya que el mercado es dinámico y la valoración de las características técnicas cambia con el tiempo. Poner un precio demasiado alto puede alejar a los clientes, mientras que ponerlo demasiado bajo puede reducir los márgenes de ganancia.

Esto motivó a CompuAlpes a proponer el objetivo de este proyecto, en el cual se desea construir un modelo de regresión que permita estimar el precio de un portátil a partir de sus especificaciones técnicas, determinando las que más impactan en el precio o que son, de acuerdo a la evidencia, irrelevantes para la estimación. Este modelo permitirá a CompuAlpes tener una base objetiva y cuantitativa al momento de establecer precios para sus productos. 

Para este fin, CompuAlpes le ha entregado los siguientes recursos:

* [Datos de entrenamiento](data/laptop_data_train.csv)
* [Datos de prueba (no etiquetados)](data/laptop_data_test_unlabeled.csv)
* [Diccionario de datos](data/diccionario.txt)

### Instrucciones

CompuAlpes desea que usted los apoye en la construcción del modelo de regresión previamente descrito utilizando algunas de las etapas de la metodología "ASUM-DM":

1. **Entendimiento de los datos:** Describir la característica más relevantes de los datos, incluir el análisis de calidad de datos y hacer una preselección de las variables más importantes para la etapa de modelado.

2. **Preparación de datos:** Solucionar cualquier problema de calidad de datos previamente identificado. Además, debe aplicar cualquier proceso de preprocesamiento de datos necesario para la construcción del modelo de regresión.

3. **Modelado:** Utilizando las variables previamente seleccionadas, construir un modelo de regresión que estime la variable objetivo con el menor error posible.

4. **Evaluación cuantitativa:** A partir de las métricas seleccionadas para evaluar y seleccionar el mejor modelo, explicar el resultado obtenido desde el punto de vista cuantitativo. Contestar a la pregunta: ¿Su equipo recomienda utlizar en producción el modelo de estimación de precios de portátiles o es preferible continuar haciendo estimaciones de forma manual? En caso de no recomendar el uso del modelo, ¿qué recomendaciones haría para continuar iterando con el objetivo de la construcción de un mejor modelo?

5. **Evaluación cualitativa:** Debe estar compuesta de dos partes:

      **- Validación de supuestos:** Realizar los ajustes necesarios para que el modelo cumpla con los supuestos necesarios para la inferencia estadística con regresiones.

      **- Interpretación de los coeficientes:** Realizar la interpretación de los coeficientes de la regresión, identificando las variables más relevantes para la estimación y como afectan la variable objetivo.

6. **Visualización de los resultados:** Integrar el resultado obtenido con el modelo de regresión a un tablero de control para apoyar el objetivo de la empresa.

7. Exportar el mejor modelo (utilizando pipelines) para poder ser usado sobre datos nuevos en el ambiente de producción del cliente.

8. Generar predicciones sobre los datos de prueba que no se encuentran etiquetados utilizando el mejor modelo. Exportar las predicciones en formato CSV utlizando como base el mismo archivo de datos de prueba.

### Construcción de pipelines

Para realizar esta sección se recomienda utilizar JupyterLab para la construcción del Pipeline y la exportación del modelo. 

El objetivo de crear un [pipeline](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html) es automatizar todos los 
pasos realizados sobre los datos, desde que salen de su fuente hasta que son ingresados al modelo de aprendizaje automático. 
Para un problema clásico, estos pasos incluyen: la selección de características o columnas, la imputación de valores no existentes, la codificación de variables categóricas utilizando diferentes técnicas como Label Encoding o One Hot Encoding, el escalamiento de variables numéricas en caso de ser necesario y en general las diferentes tareas de transformación requeridas para la preparación de los datos. Además, como último paso, el pipeline contiene el modelo que recibe los datos después de la tranformación para realizar predicciones. Finalmente, estos pipelines pueden resultar muy útiles a la hora de calibrar y comparar modelos, pues se tiene la certeza de que los datos de entrada son los mismos para todos. Incluso, pueden ser utilizados para realizar validación cruzada utilizando GridSerchCV o RandomizedSerchCV. Así mismo, pueden ser exportados para llevar los modelos a producción por medio de la serialización de estos en archivos .pkl o .joblib. 

La librería Scikit-Learn cuenta con API para la creación de pipelines en la que pueden ser utilizados diferentes pasos para la transformación de los datos que serán aplicados secuencialmente. Note que estos pasos implementan los métodos **fit** y **transform** para ser invocados desde el pipeline. Por otro lado, los modelos que serán la parte final del proceso de automatización solo cuentan con método fit. Una vez construido el modelo es posible serializar este, haciendo uso de la función **dump** de la librería joblib, para posteriormente deserializar, cargar (mediante la función **load**) y utilizar el modelo en cualquier otra aplicación o ambiente. Tenga en cuenta que la serialización de un modelo solo incluye la estructura y configuraciones realizadas sobre el pipeline, más no las instancias de los objetos que lo componen. Pues estos son provistos por la librería, por medio de la importación, en cualquiera que sea su ambiente de ejecución. Esto significa que si usted construye transformaciones personalizadas, debe incluir por separado estas, en el ambiente donde cargará y ejecutará el modelo una vez sea exportado, ya que estas no están incluidas en la serialización. 

En este punto, debe construir un pipeline que incluya todos los pasos necesarios para transformar los datos desde el archivo fuente y que estos puedan ser utilizados para realizar predicciones.

A continuación puede encontrar algunos artículos que pueden ser de utilidad para la construcción de pipelines. 
<br>
<br>
[Scikit-learn Pipeline Tutorial with Parameter Tuning and Cross-Validation](https://towardsdatascience.com/scikit-learn-pipeline-tutorial-with-parameter-tuning-and-cross-validation-e5b8280c01fb)
<br>
[Data Science Quick Tip #003: Using Scikit-Learn Pipelines!](https://towardsdatascience.com/data-science-quick-tip-003-using-scikit-learn-pipelines-66f652f26954)
<br>
[Data Science Quick Tip #004: Using Custom Transformers in Scikit-Learn Pipelines!](https://towardsdatascience.com/data-science-quick-tip-004-using-custom-transformers-in-scikit-learn-pipelines-89c28c72f22a)
<br>
[Creating custom scikit-learn Transformers](https://www.andrewvillazon.com/custom-scikit-learn-transformers/)

## <a name="entregables"></a> Entregables

* Informe del laboratorio, que puede ser el mismo notebook, con el desarrollo, análisis respetivo y la evidencia de las etapas del 1 al 5.
* Presentación con los resultados para CompuAlpes.
* Archivo fuente del tablero de control con la visualización de resultados.
* Notebook con todo el código fuente **ejecutado**.
* Modelo integrado con pipelines exportado en formato .joblib.
* Archivo de predicciones en formato CSV utilizando como base el mismo archivo de datos de prueba.
	
Se espera que el informe incluya **JUSTIFICACIONES** de las decisiones tomadas durante la construcción e interpretación de los modelos.

## Instrucciones de Entrega
- El laboratorio se entrega en grupos de mínimo 2 y máximo 3 estudiantes.
- Recuerde hacer la entrega por la sección unificada en Bloque Neón, antes del domingo 17 de septiembre a las 22:00.   
  Este será el único medio por el cual se recibirán entregas.
- No olvide indicar en el informe los aportes realizados por cada uno de los integrantes del grupo, según la propuesta hecha en la rúbrica.

## <a name="rubrica"></a> Rúbrica de Calificación

A continuación se encuentra la rúbrica de calificación:

| Concepto | Porcentaje |
|:---:|:---:|
| 1. Descripción del entendimiento de datos  | 10% |
| 2. Descripción del proceso de selección de variables | 5% |
| 3. Descripción de los procesos de limpieza y preparación de datos implementados | 15% |
| 4. **Estudiante 1:** Construcción del modelo de regresión lineal y cálculo de sus métricas de calidad  | 5% |
| 5. **Estudiante 1:** Implementación del pipeline con todas las transformaciones requeridas para la generación de predicciones, exportado en formato .joblib | 15% |
| 6. **Estudiante 2:** Exploración de los supuestos de la regresión a partir del modelo construido | 5% |
| 7. **Estudiante 2:** Desarrollo de las transformaciones complementarias para cumplir con los supuestos de la regresión e interpretación de los coeficientes del modelo | 15% |
| 8. **Estudiante 3:** Presentación para CompuAlpes con resultados a nivel cuantitativo y cualitativo del mejor modelo construido, recomendaciones dadas a la empresa y visualizaciones extraidas del notebook y/o tablero de control | 10% |
| 9. **Estudiante 3:** Tablero de control construido para la visualización de resultados del modelo | 10% |
| 10. Archivo  de predicciones sobre los datos de prueba en formato CSV | 5% |
| 11. Notebook ejecutado | 5% |
