# Declaración de uso de herramientas de IA — U02.E04 (evidencia de LAB07)

## Herramienta utilizada
Claude (Anthropic), vía Cowork, con acceso directo (lectura/escritura) a los archivos de este
proyecto a través del puente de dispositivo de Cowork, y a un entorno aislado en la nube para
probar código antes de guardarlo.

## Qué se le pidió (prompts relevantes, resumidos)
- Evaluar si el documento de LAB07 compartido correspondía al mandato de U02.E04 antes de
  proceder.
- Revisar el scaffold `INF8239_U02_CV_Proyecto_Base` (provisto por el curso, sin modificar su
  lógica de `src/inf8239_u02_cv/data.py` ni `models.py`) y detectar inconsistencias.
- Construir un notebook ejecutable adicional que reutilice ese código para comparar el baseline
  denso y la CNN bajo la misma partición (carga, EDA, entrenamiento, comparación Green AI, matriz
  de confusión, errores visuales).
- Completar `MODEL_CARD.md` con las métricas reales una vez obtenidas.
- Ante una dificultad del estudiante para redactar el cierre interpretativo del notebook (Paso 8),
  se le pidió explícitamente a la IA que propusiera un borrador de esa redacción, basado
  únicamente en los resultados reales ya generados.
- Ayudar con los comandos de entorno (`uv`), Git/GitHub y la redacción del PDF de entrega.

## Qué se verificó (no se aceptó por confianza)
- Antes de modificar nada, se instaló TensorFlow 2.21 en un entorno aislado y se ejecutó el código
  original del scaffold (`data.py`, `models.py`) contra datos sintéticos con la misma forma que
  Fashion-MNIST, confirmando que las 3 pruebas incluidas (`tests/test_data.py`,
  `tests/test_model_contract.py`) pasan tal cual se entregó el proyecto.
- El notebook adicional (`notebooks/01_vision_cnn.ipynb`) se probó celda por celda, de punta a
  punta, contra un dataset sintético (7,000 imágenes de entrenamiento, 500 de prueba) antes de
  guardarlo, confirmando que corre sin errores (carga, split train/valid/test, EDA, entrenamiento
  de ambos modelos, comparación Green AI, matriz de confusión, errores visuales, reports
  generados). Con datos sintéticos aleatorios el modelo no aprende nada real (precisión ≈ azar) —
  eso es esperado y solo confirma que el código corre, no reemplaza la ejecución real.
- La ejecución real sobre Fashion-MNIST real (descarga, entrenamiento de 8 épocas, métricas y
  figuras reales) la hizo el estudiante en su propia máquina — los números y figuras finales del
  PDF y del Model Card son resultado de esa ejecución real, no de la IA.
- Las métricas usadas para completar `MODEL_CARD.md` y el cierre interpretativo del notebook
  (F1 por modelo, parámetros, tiempos, matriz de confusión, reporte por clase) se leyeron
  directamente de los archivos reales que generó el estudiante (`reports/cv_metrics.json`,
  `reports/green_ai_comparison.csv`, `reports/confusion_cnn.png`, la salida de
  `classification_report` embebida en el notebook ejecutado) — no se inventó ningún número.

## Correcciones realizadas
- El `.gitignore` original del scaffold excluía `reports/*.png` y `reports/*.json`, lo cual
  contradice las propias instrucciones de Git del manual de LAB07, que indican explícitamente
  `git add src scripts tests reports MODEL_CARD.md uv.lock` (es decir, sí espera que `reports/` se
  suba). Se corrigió el `.gitignore` para que la evidencia visual (matriz de confusión, errores,
  métricas) quede versionada, como exige el mandato ("Presenta evidencia visual legible y
  resultados obtenidos").
- Se agregó `env/` al `.gitignore` de forma preventiva, por la misma confusión de nombre de
  entorno virtual que ocurrió en el proyecto de PLN de este estudiante.
- La celda del Paso 4 (comparación Green AI) del notebook usaba originalmente `pandas`, que nunca
  fue declarado como dependencia del proyecto (ni en el scaffold original ni en `pyproject.toml`).
  Esto causó un `ModuleNotFoundError` real al ejecutarlo en la máquina del estudiante. Se corrigió
  reescribiendo esa celda con el módulo estándar `csv`, sin agregar dependencias nuevas al
  proyecto, y se validó de nuevo contra datos sintéticos antes de reenviarla.

## Cierre interpretativo del notebook (Paso 8) — aclaración importante
El mandato exige explícitamente que esa sección sea "redacción del estudiante" y requiere
criterio humano. El estudiante tuvo dificultad para completarla y pidió un borrador. La IA
propuso un texto basado estrictamente en los números y figuras reales ya generados (sin agregar
ni inventar ningún resultado nuevo), pero esa redacción es una propuesta de la IA, no una
producción original del estudiante. Por transparencia, y porque el propio mandato exige un "uso
responsable de IA" bajo el cual el estudiante debe poder explicar cualquier fragmento entregado,
se deja indicado aquí que el estudiante debe leer, ajustar a su propio criterio y estar en
condiciones de explicar cada punto de esa sección antes de entregar — el borrador es un punto de
partida, no un sustituto del criterio propio del estudiante.

## Lo que la IA NO hizo
No se fabricó ninguna métrica, figura, archivo de reporte ni número presentado en el Model Card
o en el notebook — todo proviene de la ejecución real del estudiante. La IA tampoco decidió qué
dataset usar, ni corrió el entrenamiento real, ni alteró los resultados obtenidos para que se
vieran mejor.
