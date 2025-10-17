# Análisis de Rotación de Personal

Proyecto de análisis de rotación de empleados que integra procesamiento de lenguaje natural (NLP) con datos de recursos humanos para identificar y cuantificar los factores que influyen en la retención y rotación de personal.

## Descripción

Este proyecto implementa un pipeline de análisis que combina Latent Dirichlet Allocation (LDA) para topic modeling de respuestas textuales con análisis cuantitativo de datos de empleados. El objetivo es transformar insights cualitativos de encuestas gerenciales en métricas accionables de retención de talento.

## Objetivos

- Identificar factores estadísticamente significativos asociados con permanencia prolongada de empleados
- Analizar causas de rotación temprana mediante segmentación temporal
- Comprender patrones de salida en empleados con alta antigüedad
- Evaluar desafíos contextuales por centro de trabajo
- Cuantificar oportunidades de intervención para mejorar retención

## Fuentes de Datos

### Dataset Principal

**LDA_res.csv** (11.1 MB)
- Matriz de probabilidades de 120 tópicos extraídos de respuestas de encuestas
- Dimensiones: n × 120 (donde n = número de respuestas)
- Vinculación: número de empleado como clave primaria

**Sondeo - Perspectiva del Gerente sobre Rotación.xlsx** (1.1 MB)
- Respuestas textuales de gerentes a 6 preguntas abiertas
- Variables cualitativas sobre percepción de rotación
- Metadata: identificador de gerente, fecha, centro de trabajo

**GCP_Core_Plantilla_ActivaVSAutorizada_REP_8_Sheet1.csv** (97.7 MB)
- Base de datos transaccional de empleados
- Variables: estatus (activo/inactivo), posición, departamento, división, unidad de negocio, fechas de ingreso/salida, salario, centro de costo, familia de puesto, ubicación geográfica
- Permite análisis de cohortes y cálculo de métricas temporales

### Modelos Pre-entrenados

**modelo/topic_names.pkl** (4.6 KB)
- Mapeo de índices de tópicos a etiquetas interpretables

**modelo/topics_dict.pkl** (22.5 KB)
- Estructura jerárquica: pregunta → tópico → distribución de palabras

## Marco Metodológico

### Instrumentos de Medición

El estudio analiza 6 dimensiones de rotación mediante preguntas estructuradas:

1. **P1 - Factores de Retención**: ¿Qué crees que ha hecho que algunas personas lleven tanto tiempo en la empresa?
2. **P2 - Rotación Temprana**: ¿A qué atribuyes que algunos Colaboradores renuncien teniendo poco tiempo laborando?
3. **P3 - Rotación de Empleados Consolidados**: ¿A qué atribuyes que algunos Colaboradores renuncien teniendo mucho tiempo laborando?
4. **P4 - Factores Contextuales**: ¿Qué condiciones internas y/o del entorno hacen que liderar este centro sea más retador?
5. **P5 - Intervenciones Potenciales**: ¿Qué apoyos, decisiones o herramientas podrían ayudar a mejorar la permanencia?
6. **P6 - Variables No Capturadas**: ¿Hay algo importante que agregar para comprender por qué los Colaboradores se van?

### Técnicas Analíticas

**Natural Language Processing**
- Latent Dirichlet Allocation (LDA) con 10 tópicos por pregunta
- Preprocesamiento: tokenización, eliminación de stopwords, lematización (español)
- Extracción de temas latentes mediante inferencia bayesiana

**Análisis Estadístico**
- Cálculo de tasas de rotación:
  ```
  Tasa de Rotación (%) = (Separaciones / Promedio de Empleados) × 100
  ```
- Segmentación multidimensional: gerente, departamento, rol, cohorte de antigüedad
- Análisis de supervivencia mediante filtros temporales
- Agregaciones por período (mensual, trimestral, anual)

**Integración de Datos**
- Joins left sobre número de empleado como clave foránea
- Normalización de variables categóricas
- Imputación de valores faltantes basada en contexto organizacional

## Principales Hallazgos

### Factores de Retención (P1)
- Compromiso organizacional y sentido de responsabilidad
- Sistema de beneficios competitivo
- Cultura y ambiente laboral positivo
- Trayectorias de desarrollo profesional claras
- Estabilidad laboral y seguridad del empleo

### Causas de Rotación Temprana (P2)
- Incompatibilidad de horarios con expectativas personales
- Carga de trabajo percibida como excesiva
- Compensación no competitiva con mercado
- Desajuste entre expectativas y realidad del puesto
- Bajo compromiso inicial con la organización

### Oportunidades de Intervención (P5)
- Ajustes salariales y esquemas de incentivos basados en desempeño
- Mejora en paquete de prestaciones
- Flexibilización de horarios y modalidades de trabajo
- Programas estructurados de desarrollo y capacitación continua
- Beneficios de transporte y movilidad

## Instalación y Configuración

### Requisitos del Sistema

- Python >= 3.11
- Sistema operativo: Linux, macOS, Windows
- Memoria RAM: mínimo 8 GB (recomendado 16 GB para datasets completos)

### Instalación de Dependencias

```bash
# Clonar repositorio
git clone <repository-url>
cd rotacion

# Instalación usando uv (recomendado)
uv sync

# Alternativamente, con pip
pip install -e .
```

### Estructura del Proyecto

```
rotacion/
├── data/                           # Datasets crudos
│   ├── LDA_res.csv
│   ├── Sondeo - Perspectiva del Gerente sobre Rotación.xlsx
│   └── GCP_Core_Plantilla_ActivaVSAutorizada_REP_8_Sheet1.csv
├── modelo/                         # Artefactos de modelos
│   ├── topic_names.pkl
│   └── topics_dict.pkl
├── notebooks/                      # Análisis exploratorio
│   ├── Rotacion_Analysis.ipynb    # Notebook principal
│   └── Rotacion_Analysis_original.ipynb
├── main.py                        # Punto de entrada
├── pyproject.toml                 # Configuración del proyecto
├── uv.lock                        # Dependencias bloqueadas
└── README.md
```

### Ejecución

```bash
# Iniciar análisis en Jupyter
jupyter notebook notebooks/Rotacion_Analysis.ipynb

# Ejecutar pipeline completo
python main.py
```

## Stack Tecnológico

**Manipulación de Datos**
- pandas >= 2.3.3 - DataFrames, operaciones de merge/join, agregaciones

**Computación Numérica**
- numpy >= 2.3.4 - Operaciones vectorizadas, álgebra lineal

**Visualización**
- matplotlib >= 3.10.7 - Gráficos estadísticos, series temporales

**Adicionales** (implícitas en notebooks)
- scikit-learn - Implementación de LDA
- pickle - Serialización de modelos

## Contexto Organizacional

Este proyecto forma parte de una iniciativa de People Analytics orientada a:

1. Transformar decisiones de RRHH de intuitivas a basadas en evidencia
2. Cuantificar el ROI de intervenciones de retención
3. Identificar segmentos de alto riesgo de rotación
4. Desarrollar modelos predictivos de permanencia
5. Optimizar asignación de recursos en programas de retención

## Consideraciones Técnicas

**Privacidad de Datos**
- Los datasets contienen información personal identificable (PII)
- Aplicar anonimización antes de compartir resultados
- Cumplir con políticas de protección de datos organizacionales

**Pipeline de Modelado**
- El modelo LDA fue entrenado en proceso separado (no incluido en este repositorio)
- Parámetros de modelo: 10 tópicos por pregunta, alpha=auto, beta=auto
- Validación cruzada y perplexity no documentadas en notebooks actuales

**Limitaciones**
- Análisis descriptivo, no inferencia causal
- Sesgos potenciales en respuestas de gerentes
- Datos históricos, no necesariamente predictivos de comportamiento futuro

## Próximos Pasos

1. Implementar modelos predictivos de rotación (Survival Analysis, Cox Regression)
2. Automatizar pipeline de ETL para actualizaciones periódicas
3. Desarrollar dashboard interactivo para consumo ejecutivo
4. Validar hallazgos cualitativos mediante focus groups
5. Calcular métricas de negocio (costo de rotación, lifetime value de empleados)

## Licencia

[Especificar licencia del proyecto]

---

**Proyecto**: rotacion-encuestas
**Versión**: 0.1.0
**Python**: >= 3.11
