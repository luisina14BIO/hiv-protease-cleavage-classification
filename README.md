# Clasificación de sitios de corte de la proteasa del VIH-1

## Descripción del proyecto

Proyecto de clasificación binaria que utiliza secuencias de aminoácidos para estudiar la predicción de sitios de corte de la proteasa del VIH-1 mediante técnicas de Machine Learning.

El análisis integra cuatro conjuntos de datos publicados en la literatura científica y entrena un modelo de regresión logística para clasificar las secuencias según sus etiquetas originales.

## Objetivos

* Integrar y validar datos provenientes de distintas fuentes.
* Identificar secuencias repetidas y etiquetas contradictorias.
* Transformar las secuencias de aminoácidos en variables numéricas mediante One-Hot Encoding.
* Entrenar un modelo de regresión logística utilizando un pipeline reproducible de scikit-learn.
* Evaluar el rendimiento del modelo mediante métricas de clasificación y una matriz de confusión.

## Estructura del proyecto

```text
hiv-protease-cleavage-classification/
├── data/
│   ├── raw/
│   │   ├── 746Data.txt
│   │   ├── 1625Data.txt
│   │   ├── schillingData.txt
│   │   └── impensData.txt
│   └── processed/
├── notebooks/
│   └── modelado_metricas.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Datos
## Fuente de los datos

Los datos utilizados en este proyecto corresponden a cuatro conjuntos de secuencias de ocho aminoácidos (*octapeptides*) asociados a sitios de corte y no corte de la proteasa del VIH-1: 746, 1625, Schilling e Impens.

**Repositorio de datos:** [UCI Machine Learning Repository — HIV-1 Protease Cleavage](https://archive.ics.uci.edu/ml/datasets/HIV-1+protease+cleavage)

**Referencias científicas:**

* Rögnvaldsson, T., You, L., & Garwicz, D. (2015). *State of the art prediction of HIV-1 protease cleavage sites*. Bioinformatics, 31(8), 1204–1210. https://doi.org/10.1093/bioinformatics/btu810
* Manning, T., & Walsh, P. (2016). *The importance of physicochemical characteristics and nonlinear classifiers in determining HIV-1 protease specificity*. Bioengineered, 7(2), 65–78. https://doi.org/10.1080/21655979.2016.1149271

Los archivos originales se almacenan en `data/raw/`. Durante el análisis se mantienen las etiquetas originales y se documentan las decisiones adoptadas para tratar secuencias repetidas y etiquetas contradictorias.

Se integraron cuatro conjuntos de datos identificados como 746, 1625, Schilling e Impens.

El conjunto original integrado contiene 6.590 registros. Durante la preparación se identificaron 750 secuencias repetidas, de las cuales 10 presentaban etiquetas contradictorias entre fuentes.

Para construir el conjunto de modelado:

* Se excluyeron las 10 secuencias con etiquetas contradictorias.
* Se conservaron las secuencias repetidas con etiquetas coincidentes en una única observación.
* Se preservó la información de procedencia para mantener la trazabilidad.
* Se verificó la ausencia de secuencias duplicadas y valores faltantes.

El conjunto final contiene **5.830 secuencias únicas**, con 4.839 registros de clase -1 y 991 de clase +1.

## Metodología

### Preparación y codificación

Cada secuencia contiene ocho aminoácidos. Se utiliza One-Hot Encoding para representar cada posición mediante variables binarias, considerando los 20 aminoácidos estándar.

La representación resultante contiene 160 variables predictoras por secuencia. La columna `source` se conserva para trazabilidad, pero no se utiliza como variable predictora.

### División de los datos

Los datos se dividen en conjuntos de entrenamiento (80 %) y prueba (20 %), utilizando estratificación para mantener aproximadamente la distribución original de las clases.

Se fija `random_state=42` para facilitar la reproducibilidad de la división.

### Modelo

Se utiliza regresión logística dentro de un pipeline de scikit-learn, que integra One-Hot Encoding y clasificación.

Debido al desbalance de clases, se configura `class_weight="balanced"` durante el entrenamiento.

### Evaluación

El rendimiento se evalúa sobre el conjunto de prueba mediante:

* Accuracy.
* Precision.
* Recall.
* F1-score.
* Balanced accuracy.
* Matriz de confusión.

Se presta especial atención al recall y la precision de la clase +1, para analizar la capacidad de detectar secuencias positivas y la proporción de predicciones positivas correctas.

## Resultados

En el conjunto de prueba, compuesto por 1.166 observaciones, el modelo obtuvo los siguientes resultados:

| Métrica                  | Resultado |
| ------------------------ | --------: |
| Accuracy                 |   91,51 % |
| Balanced accuracy        |   91,67 % |
| Precision de la clase +1 |   68,68 % |
| Recall de la clase +1    |   91,92 % |
| F1-score de la clase +1  |      0,79 |

La matriz de confusión registró 885 verdaderos negativos, 182 verdaderos positivos, 83 falsos positivos y 16 falsos negativos.

El modelo identificó una proporción elevada de las secuencias positivas, aunque la precision de esa clase indica que una parte de las predicciones positivas es incorrecta.

Estos resultados corresponden a una única división de entrenamiento y prueba y deben interpretarse como una evaluación inicial del modelo.

## Requisitos

* Python 3.
* JupyterLab o Jupyter Notebook.
* Las dependencias especificadas en `requirements.txt`.

## Cómo ejecutar el proyecto

1. Clonar el repositorio:

   ```bash
   git clone (https://github.com/luisina14BIO/hiv-protease-cleavage-classification)
   ```

2. Ingresar en la carpeta del proyecto:

   ```bash
   cd hiv-protease-cleavage-classification
   ```

3. Instalar las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

4. Abrir JupyterLab:

   ```bash
   jupyter lab
   ```

5. Abrir `notebooks/modelado_metricas.ipynb` y ejecutar las celdas en orden.

El notebook utiliza rutas relativas a la estructura del proyecto. Los cuatro archivos originales deben estar disponibles en `data/raw/`.

## Referencias científicas

* Rögnvaldsson, T., You, L., & Garwicz, D. (2015). *State of the art prediction of HIV-1 protease cleavage sites*. Bioinformatics, 31(8), 1204–1210. https://doi.org/10.1093/bioinformatics/btu810

* *The importance of physicochemical characteristics and nonlinear classifiers in determining HIV-1 protease specificity* (2016). https://pmc.ncbi.nlm.nih.gov/articles/PMC4879986/
