# Red Hospitalaria Vida Integral — Analítica y Narrativa de Datos

## Tabla de Contenidos

- [Descripción General](#descripción-general)
- [Alcance del Dataset](#alcance-del-dataset)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Flujo de Trabajo y Metodología](#flujo-de-trabajo-y-metodología)
- [Hallazgos Clave y Evidencia Estadística](#hallazgos-clave-y-evidencia-estadística)
- [Recomendaciones Estratégicas](#recomendaciones-estratégicas)
- [Limitaciones del Análisis](#limitaciones-del-análisis)
- [Reproducibilidad](#reproducibilidad)
- [Autores](#autores)

## Descripción General

La **Red Hospitalaria Vida Integral** opera en **6 instituciones** de Santander:

- **Hospital Central** (Bucaramanga)
- **Clínica del Norte** (Floridablanca)
- **Hospital San Gabriel** (Piedecuesta)
- **Centro Médico Oriente** (Girón)
- **Hospital Regional** (Barrancabermeja)
- **Centro Asistencial Sur** (San Gil)[cite: 4]

El proyecto responde a una pregunta central de gestión: **¿las diferencias entre hospitales en espera, satisfacción y reingresos provienen de la institución o de los pacientes que atiende, y dónde debe intervenir primero la dirección?**[cite: 4]

## Alcance del Dataset

A partir de `Hospital_360_dataset_6000reg.xlsx` (periodo **enero de 2025 – octubre de 2026**):

- **6.000 atenciones** registradas sobre **2.930 pacientes** con atenciones (de un total de 3.600 pacientes en la dimensión).
- **8 servicios** y **10 diagnósticos**, con un promedio cercano a **2 atenciones por paciente**.
- **Costo acumulado estimado:** **$5.804 millones COP**.

## Estructura del Proyecto

``` 
Red-Hospitalaria-Vida-Integral/
├── Hospital_360_dataset_6000reg.xlsx   # Fuente de datos en múltiples hojas (fact y dimensiones)
├── Proceso_ETL.ipynb                   # Limpieza, validación, ingeniería de variables y exportación a ZIP
├── Valdiacion_Estadistica.ipynb        # Pruebas no paramétricas y exportación a Evidencia_Estadistica.xlsx
├── Proyecto integrador.docx            # Documento de narrativa en los 8 momentos y recomendaciones
└── README.md                           # Documentación principal del repositorio
```

## Flujo de Trabajo y Metodología

### 1. Limpieza, Validación y Modelado (`Proceso_ETL.ipynb`)
* Carga las hojas `fact_atenciones`, `dim_pacientes`, `dim_hospitales`, `dim_servicios`, `dim_diagnosticos`, `dim_municipios` y `dim_aseguradoras`.
* Verifica unicidad de claves primarias (PK), integridad referencial (FK sin huérfanos), rangos de triage y `satisfaccion_1_a_5`, y conserva valores nulos sin imputar (`tiempo_espera_min`: 35 nulos / 0,58 %; `costo_atencion_cop`: 12 nulos / 0,20 %; `satisfaccion_1_a_5`: 55 nulos / 0,92 %).
* Homologa nombres de municipios normalizando tildes y mayúsculas (por ejemplo, *Giron* / *Girón*).
* Construye variables derivadas y dimensiones complementarias (`dim_triage` y `dim_tiempo` diaria completa), exportando las tablas en UTF-8 con BOM dentro de `modelo_limpio.zip` junto con `bitacora_limpieza.csv`.

### 2. Contraste Estadístico (`Validación_Estadística.ipynb`)
* Une las tablas de hechos y dimensiones y calcula `espera_ajustada` como la diferencia entre el tiempo de espera de la atención y el promedio de toda la red para el mismo nivel de triage.
* Aplica pruebas no paramétricas y de asociación (Kruskal-Wallis, Chi-cuadrado, Spearman, Mann-Whitney y regresión lineal sobre medias mensuales), comparando periodos interanuales equivalentes (meses 1 a 10 de 2025 vs. 2026).
* Exporta los resultados a `Evidencia_Estadistica.xlsx`.


## Hallazgos Clave y Evidencia Estadística

* **Estabilidad temporal:** La espera promedio no muestra tendencia creciente ni cambio interanual significativo en los meses 1–10 (101,2 min en 2025 vs. 102,3 min en 2026; Mann-Whitney $p \approx 0,625$), al igual que el reingreso a 30 días en el mismo corte mensual (7,7 % vs. 8,8 %; Chi-cuadrado $p \approx 0,184$).
* **Diferencias entre hospitales tras ajustar por triage:** Existen diferencias estadísticamente significativas en espera ajustada por triage entre hospitales (Kruskal-Wallis $p < 0,001$): Hospital Regional presenta +9,8 min por encima de lo esperado y Clínica del Norte -7,8 min por debajo.
* **Priorización por triage y baja prioridad:** El tiempo de espera aumenta con niveles de menor urgencia (de ~40 min en triage 1 a ~164 min en triage 5; Spearman positivo, $p < 0,001$), y en triage 5 la satisfacción cae a 3,40.
* **Espera y satisfacción:** Se observa una asociación negativa significativa entre tiempo de espera y satisfacción (Spearman, $p < 0,001$), bajando de 4,61 (< 30 min) a 2,93 (> 180 min).
* **Costos y estancia:** Los costos varían fuertemente entre servicios (Kruskal-Wallis $p < 0,001$, promedios de ~$0,6M en Medicina general a ~$3,4M en UCI) y no entre hospitales (Kruskal-Wallis $p \approx 0,816$), con una correlación positiva alta entre estancia y costo (Spearman, $p < 0,001$).
* **Reingresos por condición clínica y edad:** El reingreso a 30 días se asocia con el diagnóstico (Chi-cuadrado $p < 0,001$), alcanzando 15,9 % en enfermedades crónicas frente a 3,8 % en el resto (y 22,1 % en adultos de 60 años o más con diagnóstico crónico), sin relación significativa con el tiempo de espera (Spearman $p \approx 0,931$) ni diferencia global significativa entre hospitales (Chi-cuadrado $p \approx 0,120$), aunque Hospital San Gabriel registra el menor reingreso (6,1 % general; 12,0 % en crónicos).
* **Complejidad institucional:** El nivel de complejidad no explica por sí solo la espera ni la satisfacción; por ejemplo, Centro Asistencial Sur (baja complejidad) promedia 97 min (-6,6 min ajustados), por debajo de Hospital Central (100 min) y Hospital Regional (112 min).

## Recomendaciones Estratégicas

* **Plan de flujo en el Hospital Regional:** Revisar procesos operativos tomando como referencia a Clínica del Norte (misma complejidad), con meta propuesta de llevar la espera ajustada de +9,8 min a ~0 min en seis meses.
* **Continuidad asistencial para pacientes crónicos y adultos mayores (60+):** Implementar seguimiento posterior al alta y documentar las prácticas de continuidad de Hospital San Gabriel, con meta propuesta de reducir el reingreso en crónicos de ~16 % hacia ~12 %.
* **Piloto en baja prioridad (triage 4 y 5):** Evaluar rutas de consulta rápida y comunicación transparente de tiempos de espera, buscando acercar los tiempos a lo logrado por Clínica del Norte (122 min en triage 4 y 154 min en triage 5).

## Limitaciones del Análisis

* **Reingresos sin antecedente en ventana:** 452 de los 489 reingresos marcados no tienen una atención previa registrada dentro de los 30 días previos en el dataset.
* **Datos faltantes:** Se conservan como nulos y se excluyen de los promedios donde aplique (35 esperas, 12 costos y 55 satisfacciones).
* **Cobertura temporal y de pacientes:** El año 2026 abarca de enero a octubre (10 meses), y 2.930 de los 3.600 pacientes de la dimensión registran atenciones.
* **Naturaleza observacional:** No se cuenta con variables de severidad clínica detallada, dotación de personal, capacidad instalada ni seguimiento ambulatorio, por lo que los resultados reflejan asociaciones y no causalidad directa. Los costos corresponden a valores estimados, no contables.

## Reproducibilidad

1. Abre `Proceso_ETL.ipynb` en Google Colab o entorno local con Python (`pandas`, `numpy`, `openpyxl`).
2. Carga `Hospital_360_dataset_6000reg.xlsx` para generar `modelo_limpio.zip` y revisar `bitacora_limpieza.csv`.
3. Ejecuta `Validación_Estadística.ipynb` (`scipy`, `pandas`, `numpy`, `openpyxl`) usando `modelo_limpio.zip` para generar `Evidencia_Estadistica.xlsx`.


## Autores

* Nayder Fabián Peña González
* Sol Valeria Arias Rojas
