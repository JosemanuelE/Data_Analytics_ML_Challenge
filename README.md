# Data Analytics ML Challenge 2026-2

**Laboratorio 1 (Semana 12).** Modelos supervisados y métricas  
**Laboratorio 2 (Semana 14).** PCA y evaluación de mejoras al modelo  
**Universidad Central** · Profesor Elias Buitrago Bolivar  
**Caso:** Brazilian E-Commerce Public Dataset by Olist

## Pregunta del caso

Con los datos que una empresa ya conoce al confirmar una compra, ¿qué pedidos tienen más riesgo de llegar después de la fecha prometida?

La variable objetivo es `is_late` (1 = entrega tardía). Una fila representa un pedido (`order_id`).

En la semana 14 la pregunta es cuál de cuatro configuraciones conviene: la que más acierta, la que detecta más tardíos o la de menor costo.

## Estado

Laboratorio S12 completo: Actividades 1 a 4, interpretación de negocio y reporte JSON. La autoverificación del cuaderno se supera ejecutando todas las celdas desde el inicio.

Laboratorio S14 completo: curva de PCA, comparación de los modelos A–D, costo, prueba KS y reporte JSON. La clase de la semana 13 no se hizo, así que la reducción de dimensionalidad quedó en este laboratorio.

**ID de estudiante:** `10` (semilla `1010`) · Huella de muestra: `094086fea2b6`  
Muestra de 35.000 pedidos: 24.500 entrenamiento, 5.250 validación y 5.250 prueba (reservada hasta la semana 16).

## Resultados de validación (5.250 pedidos)

### Semana 12

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

### Semana 14

24 variables numéricas. Para explicar al menos el 90 % de la varianza hacen falta 15 componentes (90,4 %; con 14 queda en 87,3 %). Las 4 categóricas no entran a PCA. Las cuatro regresiones usan `class_weight='balanced'` y umbral 0,50. A no usa PCA. B, C y D usan las 15 componentes.

| Modelo | PCA | C | Accuracy | Precision | Recall | F1 | AP | FP | FN | Costo |
|---|---|---|---|---|---|---|---|---|---|---|
| A | no | 1 | 0.661 | 0.122 | 0.643 | 0.205 | 0.170 | 1654 | 127 | R$ 17.677 |
| B | sí | 1 | 0.651 | 0.113 | 0.604 | 0.190 | 0.139 | 1689 | 141 | R$ 18.447 |
| C | sí | 0,1 | 0.649 | 0.113 | 0.612 | 0.191 | 0.140 | 1704 | 138 | R$ 18.462 |
| D | sí | 10 | 0.649 | 0.113 | 0.607 | 0.190 | 0.138 | 1701 | 140 | R$ 18.508 |

No hacer nada, en la misma validación, cuesta R$ 12.460.

Prueba KS de `valor_flete` entre entrenamiento y validación: D = 0.010, p = 0.769. No rechazo H0 al 5 %.

## Conclusiones principales

Semana 12:

- Elegí `'balanced'` porque con `None` el modelo casi no detecta tardíos (Recall 0.003), igual que el modelo de referencia.
- El error es mayor en agosto y marzo, y en los estados BA y RJ.
- La mayoría de los errores son falsas alarmas; con el umbral de 0,5 el modelo cuesta más que no hacer nada.
- Mejora propuesta para la semana 13: tratar `mes_compra` como variable categórica y compararla con la misma partición de validación.

El detalle está en la sección 6 del cuaderno de la semana 12.

Semana 14:

- Elegí A, sin PCA y con C = 1. Es el que más acierta, el que detecta más tardíos (229 de 356) y el de menor costo.
- PCA no mejoró el modelo. B baja el Recall de 0.643 a 0.604 y sube el costo de R$ 17.677 a R$ 18.447.
- C y D, que solo cambian la regularización, quedan casi iguales a B.
- A sigue costando más que no hacer nada, por las 1.654 falsas alarmas.
- El KS no muestra una diferencia clara en el flete. No demuestra que las distribuciones sean iguales ni que A sea mejor que B.

El detalle está en las secciones 3 a 6 del cuaderno de la semana 14.

## Cómo ejecutar

1. Abrir `notebooks/S12_ML_Challenge.ipynb` o `notebooks/S14_ML_Challenge.ipynb` en Google Colab o Jupyter.
2. **Restart & Run All** (ejecutar todas las celdas en orden desde el inicio).
3. Los CSV de Olist se descargan desde GitHub; no se suben al repositorio.

Dependencias: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `scipy`.

## Estructura del repositorio

```
notebooks/          S12_ML_Challenge.ipynb y S14_ML_Challenge.ipynb
reports/            S12_resultados_ID_10.json y S14_resultados_ID_10.json
src/                módulos de semanas siguientes
data/               no publicar CSV grandes ni la muestra de 35.000 pedidos
```

## Commits

- `S12: modelo base` — entrenamiento y métricas.
- `S12: métricas y conclusiones` — actividades 3 y 4, interpretación y JSON.
- `S14: PCA y comparacion de modelos` — curva de varianza y modelos A–D.
- `S14: costo, eleccion y prueba KS` — costo, modelo elegido, KS y JSON.
