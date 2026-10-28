# Data Analytics ML Challenge 2026-2

**Laboratorio 1 (Semana 12).** Modelos supervisados y métricas  
**Universidad Central** · Profesor Elias Buitrago Bolivar  
**Caso:** Brazilian E-Commerce Public Dataset by Olist

## Pregunta del caso

Con los datos que una empresa ya conoce al confirmar una compra, ¿qué pedidos tienen más riesgo de llegar después de la fecha prometida?

La variable objetivo es `is_late` (1 = entrega tardía). Una fila representa un pedido (`order_id`).

## Estado del avance

Este commit registra el **modelo base** (Actividades 1 y 2).

| Parte | Estado |
|---|---|
| Carga y preparación (plantilla) | Hecho |
| Actividad 1. Entrenamiento y elección de `class_weight` | Hecho |
| Actividad 2. Métricas y matriz de confusión | Hecho |
| Actividad 3. Errores por segmento | Pendiente |
| Actividad 4. Costo empresarial | Pendiente |
| Sección 6. Interpretación de negocio | Pendiente |
| JSON en `reports/` | Pendiente |

**ID de estudiante:** `10` (semilla `1010`).

## Resultados de validación (5.250 pedidos)

Modelo: regresión logística en un `Pipeline`, `class_weight='balanced'`.

| Métrica | Valor |
|---|---|
| Accuracy | 0.661 |
| Precision | 0.122 |
| Recall | 0.643 |
| F1 | 0.205 |
| ROC-AUC | 0.708 |
| AP | 0.170 |

Matriz de confusión: TN 3240 · FP 1654 · FN 127 · TP 229.

Elegí `'balanced'` porque con `None` la Accuracy se parece a la del Dummy (~0.93) y el Recall de tardíos queda casi en 0. `'balanced'` detecta más retrasos a costa de más falsas alarmas.

## Cómo ejecutar

1. Abrir `notebooks/S12_ML_Challenge.ipynb` en Google Colab o Jupyter.
2. **Restart & Run All** (o ejecutar las celdas en orden).
3. Los CSV de Olist se descargan desde GitHub; no se suben al repositorio.

Dependencias: `pandas`, `numpy`, `scikit-learn`.

## Estructura del repositorio

```
notebooks/          cuaderno de análisis de la semana 12
reports/            JSON de resultados (cuando se cierre el laboratorio)
src/                módulos de semanas siguientes
data/               no publicar CSV grandes ni la muestra de 35.000 pedidos
```

## Commits previstos

- `S12: modelo base` — este avance (entrenamiento y métricas).
- `S12: métricas y conclusiones` — actividades 3 y 4, interpretación y JSON.

La partición de prueba no se usa hasta la semana 16.
