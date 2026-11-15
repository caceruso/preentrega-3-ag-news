# Preentrega 3 - Clasificación de noticias con AG News

Autor: Carlos Ruiz

## Objetivo

Clasificar noticias de AG News en cuatro categorías: World, Sports,
Business y Sci_Tech. Reutilicé las funciones de limpieza y
lematización del módulo 2.

## Cómo ejecutar el notebook

1. Abre la `Pre_Entrega_3.ipynb` en Google Colab.
2. Ejecutá la primera celda y subí los archivos oficiales
   `ag_news_train.csv` y `ag_news_test.csv`.
3. Ejecutá las demás celdas en orden. El notebook instala spaCy
   y descarga el modelo de inglés `en_core_web_sm`.

## Entrenamiento y selección del modelo

Elegí la regresión logística porque puede trabajar directamente
con las matrices dispersas de TF-IDF y sirve como modelo de
referencia para comparar distintas formas de representar el texto.

Dividí las 8.000 noticias del conjunto oficial de entrenamiento en
6.400 para ajustar el modelo y 1.600 para validación. Usé una
división estratificada con semilla 42 para mantener la proporción
de las cuatro categorías y poder repetir la comparación.

Probé ocho configuraciones con límites de 10.000 y 20.000 términos,
unigramas o unigramas y bigramas, y con o sin filtro de stop-words.
En cada prueba ajusté TF-IDF con `fit_transform()` sobre las 6.400
noticias de entrenamiento y apliqué `transform()` a la validación.

El modelo base obtuvo un F1 macro de 0,8856 en validación. La
configuración elegida obtuvo 0,8941 con `max_features=10000`,
`ngram_range=(1, 2)` y un filtro de stop-words de spaCy que conserva
negaciones y `us`.

## Evaluación en el test oficial

Después de elegir la configuración, volví a ajustar TF-IDF y la
regresión logística con las 8.000 noticias de entrenamiento. Apliqué
la misma limpieza a las 2.000 noticias de prueba y las vectoricé
con `transform()` usando el vocabulario aprendido en entrenamiento.

**Exactitud: 89,45 %. F1 macro: 0,8946.**

| Categoría | Precisión | Recall | F1 | Noticias |
|---|---:|---:|---:|---:|
| World | 0,9181 | 0,8740 | 0,8955 | 500 |
| Sports | 0,9545 | 0,9660 | 0,9602 | 500 |
| Business | 0,8385 | 0,8620 | 0,8501 | 500 |
| Sci_Tech | 0,8690 | 0,8760 | 0,8725 | 500 |

Sports obtuvo el F1 más alto y Business el más bajo. En la matriz
de confusión del notebook, 42 noticias de Business se clasificaron
como Sci_Tech y 46 de Sci_Tech como Business. Esas dos confusiones
representaron 88 de los 211 errores del modelo.

## Dependencias

Versiones usadas en Google Colab:

- `pandas==2.2.3`
- `scikit-learn==1.6.1`
- `spacy==3.8.18`
- `matplotlib==3.10.0`
- Modelo de spaCy `en_core_web_sm==3.8.0`
