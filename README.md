# FitLife - Análisis de abandono de socios

Análisis de abandono de socios de un gimnasio ficticio para identificar perfiles de riesgo y proponer acciones de retención basadas en datos.

![Resumen ejecutivo](dashboard\screenshots\Resumen_ejecutivo.png)

## Resultados clave

- Tasa global de abandono: **26,53%** (1.061 bajas sobre 4.000 socios).
- Los contratos mensuales abandonan el doble: **42,32%**.
- Coste estimado del abandono: **52.920 €** (hipótesis de 30 €/mes).

**Herramientas:** Python · SQL · Power BI · scikit-learn

**Informe completo:** [Informe_Final_FitLife.pdf](Informe_Final_FitLife.pdf)

## Objetivo del proyecto

FitLife necesita comprender por qué algunos socios abandonan el gimnasio y qué acciones podrían ayudar a reducir la pérdida de clientes. Para ello se realiza un proceso completo de analítica de datos:

- limpieza y preparación del dataset;
- enriquecimiento de variables de negocio;
- análisis exploratorio de abandono;
- modelado SQL en esquema estrella;
- dashboard interactivo en Power BI;
- aplicación de regresión logística;
- conclusiones y recomendaciones accionables.

## Preguntas de negocio

- ¿Cuál es la tasa global de abandono?
- ¿Qué tipo de contrato presenta mayor riesgo?
- ¿Los socios nuevos abandonan más que los socios veteranos?
- ¿La asistencia a clases grupales se asocia con mayor retención?
- ¿Qué perfiles de socios activos deberían priorizarse en campañas preventivas?

## Principales resultados

- Tasa global de abandono: **26,53%**.
- Total de bajas analizadas: **1.061 socios** sobre 4.000.
- Los contratos mensuales presentan la mayor tasa de abandono: **42,32%**.
- Los socios nuevos tienen una tasa de abandono del **42,14%**.
- Los socios que no asisten a clases grupales abandonan más: **33,01%** frente a **17,28%**.
- Los socios sin pareja socia presentan mayor abandono: **33,32%** frente a **19,36%**.
- El coste estimado del abandono es de **52.920 €**, bajo la hipótesis de cuota media de 30 €/mes.
- Una reducción del abandono del 20% supondría un ahorro potencial estimado de **10.584 €**.

## Estructura del repositorio

```text
FitLife/
├── dashboard/
│   ├── powerbi/
│   │   └── Proyecto_FitLife_final.pbix
│   └── screenshots/
│       ├── Resumen ejecutivo.png
│       ├── Factores de riesgo.png
│       └── Recomendaciones.png
├── data/
│   ├── raw/
│   │   └── gym_churn_us.csv
│   └── processed/
│       ├── gym_churn_enriquecido.csv
│       └── modelo_estrella/
│           ├── dim_actividad.csv
│           ├── dim_captacion.csv
│           ├── dim_contrato.csv
│           ├── dim_socio.csv
│           └── fact_membresia.csv
├── docs/
│   └── img/
├── notebooks/
│   ├── 01_exploracion_inicial.ipynb
│   ├── 02_limpieza_enriquecimiento.ipynb
│   ├── 03_analisis_exploratorio.ipynb
│   └── 04_modelo_regresion_logistica.ipynb
└── sql/
    ├── 01_crear_tablas.sql
    └── 02_queries_y_kpis.sql
```

## Dataset

El dataset original contiene **4.000 registros** y **14 columnas** relacionadas con características demográficas, contractuales y de actividad de socios de gimnasio.

Durante la limpieza se detectó un problema de exportación en tres columnas:

- `Avg_additional_charges_total`
- `Avg_class_frequency_total`
- `Avg_class_frequency_current_month`

Estas variables aparecían como texto con múltiples separadores decimales y no podían reconstruirse de forma fiable. Por ese motivo se excluyeron del análisis para evitar conclusiones basadas en datos corruptos.

El dataset procesado contiene **4.000 registros** y **17 columnas**, incluyendo variables enriquecidas como:

- `grupo_edad`
- `tipo_contrato`
- `nivel_compromiso`
- `perfil_captacion`
- `segmento_lifetime`

## Modelo SQL

El proyecto incluye un modelo estrella simple compuesto por:

- `dim_socio`
- `dim_contrato`
- `dim_captacion`
- `dim_actividad`
- `fact_membresia`

Los scripts SQL incluyen:

- creación de tablas;
- relaciones mediante claves foráneas;
- consultas con `SELECT`, `JOIN`, `GROUP BY` y subconsultas;
- KPIs de negocio como tasa de abandono, vida media antes de baja, socios activos en riesgo, coste estimado y ahorro potencial.

**Fuente:** [Gym customers features and churn](https://www.kaggle.com/datasets/adrianvinueza/gym-customers-features-and-churn). FitLife es una empresa ficticia usada con fines formativos.

## Dashboard Power BI

Contiene tres páginas principales:

1. **Resumen ejecutivo**: KPIs generales, abandono por contrato y abandono por antigüedad.
2. **Factores de riesgo**: análisis por edad, contrato, clases, captación y pareja socia.
3. **Recomendaciones**: interpretación del modelo y priorización de socios activos por riesgo.

![Factores de riesgo](dashboard\screenshots\Factores_riesgo.png)
![Recomendaciones](dashboard\screenshots\Recomendaciones.png)

## Técnica analítica aplicada

Se aplicó una **regresión logística** para estimar la probabilidad de abandono.

Variables utilizadas:

- duración del contrato;
- edad;
- asistencia a clases grupales;
- promoción por amigos;
- cercanía al gimnasio;
- teléfono registrado;
- pareja socia;
- género.

Variables como `Lifetime`, `Month_to_end_contract` y `segmento_lifetime` se excluyeron del modelo predictivo para reducir el riesgo de fuga de información.

Métricas principales del modelo:

- Accuracy: **0.805**
- Precision: **0.697**
- Recall: **0.467**
- F1-score: **0.559**
- ROC AUC: **0.871**

El modelo se utiliza como apoyo interpretativo para priorizar acciones de retención, no como sistema automático de decisión.

## Recomendaciones de negocio

1. Reforzar el onboarding durante los primeros tres meses.
2. Incentivar el paso de contrato mensual a semestral o anual.
3. Promover clases grupales entre socios con baja vinculación.
4. Contactar de forma prioritaria con socios activos clasificados como riesgo alto.
5. Potenciar campañas de referidos y acciones de comunidad.
6. Monitorizar periódicamente churn, asistencia y segmentos de riesgo.

## Herramientas utilizadas

- Python
- pandas
- matplotlib
- seaborn
- scikit-learn
- SQL
- Power BI
- GitHub


## Limitaciones

- No se dispone de una serie temporal real de asistencia mes a mes.
- No hay datos reales de ingresos por socio; el cálculo económico usa una hipótesis de 30 €/mes.
- Tres columnas fueron descartadas por corrupción de formato.
- El modelo identifica asociaciones, no causalidad demostrada.
