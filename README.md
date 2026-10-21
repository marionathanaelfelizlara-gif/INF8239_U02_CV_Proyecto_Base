# INF-8239 · Unidad 02 · Proyecto de visión

Autor académico: Mario Nathanael Feliz Lara

## CPU
```bash
uv python install 3.12
uv sync --extra cpu
uv run python scripts/check_runtime.py
uv run python scripts/train_cv.py --epochs 8
uv run pytest -q
```

## GPU
La ruta GPU se utiliza solamente en WSL2/Linux con NVIDIA correctamente configurada:
```bash
uv sync --extra gpu
```

## Notebook ejecutable

`notebooks/01_vision_cnn.ipynb` integra, de forma interactiva, la carga de Fashion-MNIST, el
análisis exploratorio, el entrenamiento y comparación del baseline denso y la CNN (bajo una
óptica Green AI: F1 macro, parámetros, tiempo de entrenamiento e inferencia), la matriz de
confusión y los errores visuales. Reutiliza el código de `src/inf8239_u02_cv/`, no duplica
lógica. Córralo con:

```bash
uv add --dev jupyter
uv run jupyter nbconvert --to notebook --execute --inplace notebooks/01_vision_cnn.ipynb
```

Después de ejecutarlo, complete a mano el cierre interpretativo del último paso del notebook
(requiere su criterio, no se completa solo) y luego `MODEL_CARD.md` con las métricas reales de
`reports/cv_metrics.json` y `reports/green_ai_comparison.csv`.

## Requirements

Después de `uv sync --extra cpu`, genere el `requirements.txt` exigido por el mandato:

```bash
uv export --format requirements.txt --no-dev --output-file requirements.txt
```

## Declaración de uso de IA

`docs/USO_DE_IA.md` — herramienta usada, qué se le pidió, qué se verificó con pruebas y
ejecución real (no por confianza), y qué correcciones se le hicieron.

Complete `MODEL_CARD.md` después de analizar métricas, errores y costo.
