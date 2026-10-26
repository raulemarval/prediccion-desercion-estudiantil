# Predicción de deserción estudiantil mediante Machine Learning

**Proyecto Final Integrador — Diplomatura en Ciencia de Datos y Análisis Avanzado, UTN.BA Centro de e-Learning**  
**Autor individual:** Raúl Enrique Marval Palacios  
**Octubre de 2026**

> Los datos no reemplazan nuestra mirada, pero pueden ayudarnos a llegar a tiempo.

## Objetivo

Explorar si la información disponible hasta el primer semestre permite reconocer señales asociadas con la deserción estudiantil. Se comparan **Regresión Logística** y **Random Forest** mediante validación cruzada, priorizando el *recall* de abandono. Este ejercicio retrospectivo **no demuestra** que las predicciones reduzcan la deserción.

## Fuente de datos

UCI Machine Learning Repository, [Predict Students’ Dropout and Academic Success (ID 697)](https://archive.ics.uci.edu/dataset/697/predict+students+dropout+and+academic+success). El notebook descarga el archivo desde UCI; **no se incluye el dataset en este repositorio**. El conjunto original tiene 4.424 registros y 37 columnas, con 2.209 casos *Graduate*, 1.421 *Dropout* y 794 *Enrolled*. No se encontraron valores faltantes ni filas duplicadas exactas en la revisión realizada.

Para la clasificación binaria se excluye *Enrolled*, porque aún no tiene resultado definitivo: quedan 3.630 estudiantes. Se excluyen las variables explícitas del segundo semestre para aproximar una predicción al cierre del primero. **Esta selección temporal debe auditarse antes de cualquier uso institucional**.

## Archivos

- `analisis_desercion.ipynb`: código reproducible, EDA, preparación, baseline, comparación de modelos y evaluación.
- `requirements.txt`: bibliotecas necesarias.
- `resultados/`: métricas CSV obtenidas en una ejecución previa de Google Colab.
- `figuras/`: gráficos del análisis exploratorio.
- `.gitignore`: archivos locales que no deben publicarse.

## Ejecución en Google Colab

1. Abrir [Google Colab](https://colab.research.google.com/).
2. Subir `analisis_desercion.ipynb` desde **Archivo → Subir notebook**.
3. Conectar el entorno y elegir **Entorno de ejecución → Ejecutar todas**.
4. Esperar a que se descargue el conjunto público de UCI (requiere internet).
5. Consultar los gráficos y la tabla de comparación. Los CSV se crean en `resultados/`; los gráficos nuevos, en `figuras/`.

## Ejecución local

Con Python 3.10 o posterior:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook analisis_desercion.ipynb
```

Ejecutar las celdas en orden. Los resultados pueden variar si cambian los datos, versiones de bibliotecas o condiciones de ejecución.

## Resultados de la ejecución documentada

En el conjunto de prueba de **726 estudiantes**, la Regresión Logística obtuvo:

| Métrica | Resultado |
|---|---:|
| Accuracy | 88,71 % |
| Precision (abandono) | 83,67 % |
| Recall (abandono) | 88,38 % |
| F1-score | 85,96 % |
| ROC-AUC | 94,61 % |

Matriz de confusión: 393 graduados correctamente identificados, 49 graduados señalados como abandono, 33 abandonos no detectados y 251 abandonos correctamente identificados. En validación cruzada, el *recall* promedio fue **83,38 %** para Regresión Logística y **80,39 %** para Random Forest. Estos números corresponden a la ejecución previamente realizada y se conservan en `resultados/` para su revisión.

## Método y límites

Se aplica CRISP-DM: comprensión del problema, comprensión y preparación de datos, modelado, evaluación y propuesta de uso responsable. Se usa partición estratificada 80/20, validación cruzada de cinco particiones sobre entrenamiento y evaluación final sobre test. El umbral inicial de clasificación es 0,50.

Los datos describen asociaciones, no relaciones causales. El modelo no debe emplearse para sancionar, excluir o decidir automáticamente sobre estudiantes. Antes de un piloto deben revisarse sesgos, privacidad, disponibilidad temporal de variables y desempeño en cohortes recientes.
