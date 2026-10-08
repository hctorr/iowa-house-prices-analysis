# 🏠 Modelización estadística de precios de viviendas en Ames, Iowa

Análisis estadístico completo del dataset [House Prices](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) de Kaggle (1.460 viviendas, 80 variables), desde ANOVA y regresión lineal hasta GLM, modelos penalizados, GAM y modelos mixtos con el barrio como efecto aleatorio. Hecho en R con R Markdown.

## Objetivo

Explicar y predecir el precio de venta de una vivienda (`SalePrice`) y comparar distintas familias de modelos estadísticos sobre el mismo problema. Fue un trabajo hecho para la asignatura de Modelos de regresión en un Grado en Ingeniería en Inteligencia Artificial en la UPV.

## Datos

- **Fuente:** [Kaggle · House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques), `train.csv`.
- **Tamaño:** 1.460 viviendas y 80 variables (físicas, de calidad y de contexto).
- **Variable respuesta:** `SalePrice` y `log(SalePrice)`. Para el GLM logístico, `PrecioAlto` (precio por encima de la mediana).
- **Variable de agrupación (efecto aleatorio):** `Neighborhood`.

## Contenido del análisis

| Bloque | Qué se hace |
|---|---|
| Exploración | Distribución del precio, relación con calidad general y superficie habitable |
| ANOVA | Un factor (calidad de cocina, aire acondicionado, capacidad del garaje), bifactorial con interacción y test post-hoc de Tukey |
| Regresión lineal | Modelo simple y múltiple, diagnóstico de asunciones, multicolinealidad (VIF) |
| Corrección y validación | Transformación logarítmica, outliers influyentes (distancia de Cook), comparación por R² ajustado, AIC y RMSE; validación con holdout 70/30, validación cruzada 10-fold y bootstrap |
| GLM | Regresión logística (precio alto/normal: odds ratios, test de razón de verosimilitud, curva ROC) y regresión de Poisson (nº de habitaciones, sobredispersión) |
| Modelos avanzados | Ridge, Lasso y Elastic Net (`glmnet`), GAM (`mgcv`) y modelos mixtos con `lmer` (intercepto aleatorio frente a intercepto + pendiente aleatoria por barrio) |

## Resultados

| Modelo | Métrica | Valor |
|---|---|---|
| LM con `log(SalePrice)` | R² ajustado | [valor] |
| LM con `log(SalePrice)` | RMSE validación (holdout / CV-10 / bootstrap) | [valores] |
| GLM logístico | AUC | [valor] |
| Mixto, intercepto aleatorio | AIC · RMSE | [valores] |
| Mixto, intercepto + pendiente aleatoria | AIC · RMSE | [valores] |

**Conclusiones principales:**

*ANOVA*
- **Calidad de cocina (`KitchenQual`):** las diferencias de precio entre niveles son estadísticamente significativas. Tukey confirma que casi todos los pares de niveles difieren entre sí, y las cocinas *Excelentes* se asocian a precios notablemente más altos.
- **Aire acondicionado central (`CentralAir`):** las viviendas con A/C central tienen un precio medio muy superior, con diferencia significativa aunque solo haya dos grupos.
- **Capacidad del garaje (`GarageCars`):** a mayor capacidad, mayor precio medio. Los mayores saltos ocurren entre 0 y 1 coche y entre 1 y 2.
- **Interacción cocina × A/C:** el efecto de la calidad de cocina es consistente con y sin A/C; la magnitud varía ligeramente, lo que sugiere una interacción débil.

## Limitaciones

- Datos observacionales de una sola ciudad: las relaciones son asociaciones, no causalidad.
- La mayoría de modelos usan un subconjunto de predictores, no las 80 variables.
- La curva ROC del GLM logístico se calcula sobre los datos de ajuste; la validación fuera de muestra se reporta aparte.
- `PrecioAlto` se define con la mediana del precio, así que es un problema ilustrativo y relativamente fácil.
- El RMSE de los modelos mixtos se calcula con los residuos del ajuste, no con datos de test.

## Posibles mejoras

- Validar los modelos mixtos fuera de muestra, por ejemplo dejando barrios completos fuera.
- Comparar con modelos de árboles (random forest, gradient boosting).
- Ampliar el conjunto de predictores y usar `test.csv` para una evaluación independiente.

## Cómo reproducirlo

1. Descarga `train.csv` desde [Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data) y colócalo en la misma carpeta que el `.Rmd` (no se incluye en el repositorio por las condiciones de Kaggle).
2. Instala los paquetes:

```r
install.packages(c("tidyverse", "car", "scales", "corrplot", "lmtest", "boot",
                   "pROC", "glmnet", "mgcv", "lme4", "lmerTest",
                   "ggeffects", "gridExtra"))
```

3. Abre el `.Rmd` en RStudio y pulsa **Knit** para generar el informe HTML.

## Estructura del proyecto

```
├── analisis_ames_housing.Rmd
├── data_description.txt
├── train.csv        (descargar de Kaggle)
└── README.md
```

## Tecnologías

R · R Markdown · tidyverse · lme4 · mgcv · glmnet · pROC

## Autor

**Héctor Ruiz** · [GitHub](https://github.com/hctorr) · [LinkedIn](https://www.linkedin.com/in/hectorruizgaldon/)
