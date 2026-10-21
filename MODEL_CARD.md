# Model Card · Fashion-MNIST CNN

> Borrador generado por IA a partir de resultados reales (`reports/cv_metrics.json`,
> `reports/confusion_cnn.png`, `reports/green_ai_comparison.csv`). Las secciones de juicio
> (Uso previsto, Usos fuera de alcance, Limitaciones y riesgos, Supervisión y monitoreo) son una
> propuesta razonable, no un hecho verificado por un tercero — revísalas y ajústalas con tu propio
> criterio antes de entregar, tal como pide el mandato.

## Modelo y versión

Dos modelos comparados sobre la misma partición de Fashion-MNIST, definidos en
`src/inf8239_u02_cv/models.py` y entrenados con `scripts/train_cv.py --epochs 8`
(TensorFlow/Keras 2.21.0, CPU, Windows 11, sin GPU):

- `dense_baseline` (`build_dense`): red densa de una capa oculta.
- `small_cnn` (`build_cnn`): 2 bloques Conv2D + MaxPooling2D, GlobalAveragePooling2D, Dropout.

Checkpoint entregado: `models/best_cnn.keras` (262 KB en disco). El baseline denso no se
serializa en este proyecto, solo se reportan sus métricas.

## Uso previsto

Fines didácticos: comparar, bajo un enfoque Green AI, el costo computacional y el desempeño de
dos arquitecturas distintas sobre un benchmark académico de clasificación de imágenes
(Fashion-MNIST, 10 clases de prendas). No es un clasificador listo para producción.

## Usos fuera de alcance

Fashion-MNIST son imágenes de 28×28 en escala de grises, centradas y recortadas, sin fondo,
variación de iluminación ni oclusión. Ninguno de los dos modelos debe usarse para clasificar
fotos reales de ropa (e-commerce, moda), imágenes médicas, OCR, vigilancia u otro dominio sin
reentrenamiento y validación específica sobre datos de ese dominio.

## Dataset y particiones

Fashion-MNIST estándar vía `tensorflow.keras.datasets` (70,000 imágenes, 10 clases). Partición
usada (`notebooks/01_vision_cnn.ipynb`, Paso 1): train = primeras 54,000 filas del set de
entrenamiento, valid = últimas 6,000, test = las 10,000 de prueba originales (1,000 por clase).

## Preprocesamiento

Normalización de píxeles a [0,1] (`normalize_images`) y validación de forma/rango
(`validate_images`), ambas en `src/inf8239_u02_cv/data.py`. Sin aumentos de datos (data
augmentation).

## Métricas globales y por clase

Métricas globales reales (`reports/cv_metrics.json`, test set, 10,000 imágenes):

| Modelo | F1 macro | Accuracy* |
|---|---|---|
| dense_baseline | 0.864 | — |
| small_cnn | 0.789 | 0.793 |

*Accuracy global de la CNN tomada del `classification_report` de `train_cv.py`.

Precisión/recall/F1 por clase de la CNN, derivados de la matriz de confusión real
(`reports/confusion_cnn.png`, 1,000 imágenes de prueba por clase):

| Clase | Precisión | Recall | F1 |
|---|---|---|---|
| 0 — Camiseta/top | 0.646 | 0.805 | 0.717 |
| 1 — Pantalón | 0.990 | 0.929 | 0.958 |
| 2 — Suéter | 0.702 | 0.649 | 0.674 |
| 3 — Vestido | 0.753 | 0.845 | 0.796 |
| 4 — Abrigo | 0.659 | 0.670 | 0.665 |
| 5 — Sandalia | 0.951 | 0.883 | 0.916 |
| 6 — Camisa | 0.501 | 0.364 | 0.422 |
| 7 — Zapatilla | 0.854 | 0.953 | 0.901 |
| 8 — Bolso | 0.916 | 0.937 | 0.926 |
| 9 — Botín | 0.934 | 0.896 | 0.915 |

La clase 6 ("Camisa") es, por amplio margen, la más difícil para la CNN: según la matriz de
confusión, de las 1,000 imágenes reales de "Camisa", 274 se confundieron con "Camiseta/top", 135
con "Abrigo" y 128 con "Suéter" — las cuatro son prendas superiores visualmente similares en
28×28 gris.

## Comparación de costo

- Hardware: CPU (sin GPU), Windows 11, TensorFlow 2.21.0.
- Parámetros: `dense_baseline` 50,890 — `small_cnn` 19,466.
- Tiempo de entrenamiento (8 épocas): `dense_baseline` 9.68 s — `small_cnn` 132.48 s (~13.7× más
  lento).
- Tiempo de inferencia: `dense_baseline` 0.051 ms/imagen — `small_cnn` 0.238 ms/imagen (~4.6× más
  lento).

En esta corrida, el modelo más simple (`dense_baseline`) obtuvo mejor F1 macro que la CNN y
costó mucho menos en tiempo de entrenamiento e inferencia, pese a tener más parámetros. La
mejora no justifica el costo: la CNN no mejora el resultado y es más cara en los dos ejes
medidos.

## Limitaciones y riesgos

- El resultado anterior (denso > CNN en F1 y en costo) es específico de este presupuesto de 8
  épocas y de esta arquitectura (`GlobalAveragePooling2D` reduce agresivamente la información
  espacial antes de la capa final). No debe generalizarse como "las CNN nunca superan a las
  redes densas"; se documenta tal cual se obtuvo, sin ajustar el experimento después de verlo.
- Ambos modelos comparten la misma debilidad de fondo: la clase "Camisa" se confunde con otras
  prendas superiores (ver matriz de confusión).
- Fashion-MNIST no representa la variabilidad de iluminación, fondo, oclusión ni resolución de
  imágenes del mundo real; un buen resultado aquí no demuestra que el modelo esté listo para otro
  dominio.

## Supervisión y monitoreo

Antes de reusar cualquiera de los dos modelos fuera de este ejercicio, se requiere reevaluación
con datos reales del dominio destino y revisión humana de los casos de error, en particular en
clases visualmente ambiguas como la 6 ("Camisa").
