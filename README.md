# Data Analytics ML Challenge 2026-2

**Laboratorio 1 (Semana 12).** Modelos supervisados y métricas  
**Universidad Central** · Profesor Elias Buitrago Bolivar  
**Caso:** Brazilian E-Commerce Public Dataset by Olist

## Pregunta del caso

Con los datos que una empresa ya conoce al confirmar una compra, ¿qué pedidos tienen más riesgo de llegar después de la fecha prometida?

La variable objetivo es `is_late` (1 = entrega tardía). Una fila representa un pedido (`order_id`).

## Estado

Laboratorio S12 completo: Actividades 1 a 4, interpretación de negocio y reporte JSON. La autoverificación del cuaderno se supera ejecutando todas las celdas desde el inicio.

**ID de estudiante:** `10` (semilla `1010`) · Huella de muestra: `094086fea2b6`  
Muestra de 35.000 pedidos: 24.500 entrenamiento, 5.250 validación y 5.250 prueba (reservada hasta la semana 16).

## Resultados de validación (5.250 pedidos)

Modelo: regresión logística en un `Pipeline`, `class_weight='balanced'`.

| Métrica | Modelo de referencia | Regresión logística |
|---|---|---|
| Accuracy | 0.932 | 0.661 |
| Precision | — | 0.122 |
| Recall | 0 | 0.643 |
| F1 | 0 | 0.205 |
| ROC-AUC | — | 0.708 |
| AP | — | 0.170 |

Matriz de confusión: TN 3240 · FP 1654 · FN 127 · TP 229.

Costo de errores (FP = R$ 8, FN = R$ 35): R$ 17.677 con el modelo frente a R$ 12.460 sin hacer nada.

## Conclusiones principales

- Elegí `'balanced'` porque con `None` el modelo casi no detecta tardíos (Recall 0.003), igual que el modelo de referencia.
- El error es mayor en agosto y marzo, y en los estados BA y RJ.
- La mayoría de los errores son falsas alarmas; con el umbral de 0,5 el modelo cuesta más que no hacer nada.
- Mejora propuesta para la semana 13: tratar `mes_compra` como variable categórica y compararla con la misma partición de validación.

El detalle está en la sección 6 del cuaderno.

## Cómo ejecutar

1. Abrir `notebooks/S12_ML_Challenge.ipynb` en Google Colab o Jupyter.
2. **Restart & Run All** (ejecutar todas las celdas en orden desde el inicio).
3. Los CSV de Olist se descargan desde GitHub; no se suben al repositorio.

Dependencias: `pandas`, `numpy`, `scikit-learn`.

## Estructura del repositorio

```
notebooks/          cuaderno de análisis de la semana 12
reports/            S12_resultados_ID_10.json
src/                módulos de semanas siguientes
data/               no publicar CSV grandes ni la muestra de 35.000 pedidos
```

## Commits

- `S12: modelo base` — entrenamiento y métricas.
- `S12: métricas y conclusiones` — actividades 3 y 4, interpretación y JSON.
