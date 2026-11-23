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

```text
Red-Hospitalaria-Vida-Integral/
├── Hospital_360_dataset_6000reg.xlsx   # Fuente de datos en múltiples hojas (fact y dimensiones)
├── Proceso_ETL.ipynb                   # Limpieza, validación, ingeniería de variables y exportación a ZIP
├── Valdiacion_Estadistica.ipynb        # Pruebas no paramétricas y exportación a Evidencia_Estadistica.xlsx
├── Proyecto integrador.docx            # Documento de narrativa en los 8 momentos y recomendaciones
└── README.md                           # Documentación principal del repositorio
