# Metodologia de Topic Modeling para Analisis de Rotacion

## 1. Introduccion

Este documento describe la metodologia utilizada para realizar topic modeling sobre las respuestas de gerentes en el sondeo de rotacion de personal. El objetivo es identificar temas latentes en las respuestas abiertas de las 6 preguntas del cuestionario mediante el algoritmo Latent Dirichlet Allocation (LDA).

## 2. Configuracion del Entorno

### 2.1 Librerias Requeridas

El analisis requiere las siguientes herramientas:

- **pandas**: Para manipulacion y analisis de datos estructurados
- **nltk**: Framework de procesamiento de lenguaje natural para preprocesamiento de texto
- **gensim**: Biblioteca especializada en modelado de topics y analisis semantico
- **re**: Modulo de expresiones regulares para limpieza de texto

### 2.2 Recursos de Procesamiento de Lenguaje Natural

Se utilizan tres recursos especializados de NLTK:

- **wordnet**: Base de datos lexica que permite la lematizacion de palabras
- **stopwords**: Lista de palabras comunes en español que no aportan significado (ej. "el", "la", "de")
- **punkt**: Tokenizador de texto que separa oraciones y palabras

## 3. Preprocesamiento de Texto

### 3.1 Proceso de Limpieza y Normalizacion

El preprocesamiento transforma el texto crudo en una representacion estructurada mediante cuatro etapas secuenciales:

**1. Normalizacion de Texto**
- Conversion de todo el texto a minusculas para garantizar uniformidad
- Eliminacion de variaciones por uso de mayusculas/minusculas

**2. Tokenizacion**
- Extraccion de palabras individuales del texto continuo
- Filtrado de palabras con longitud minima de 2 caracteres
- Eliminacion de caracteres no alfabeticos (numeros, signos de puntuacion)

**3. Eliminacion de Stopwords**
- Remocion de palabras comunes que no aportan significado contextual
- Ejemplos: articulos (el, la), preposiciones (de, en, para), conjunciones (y, o)
- Utiliza lista especializada en español

**4. Lematizacion**
- Reduccion de palabras a su forma base o raiz
- Ejemplos: "trabajando" → "trabajo", "empresas" → "empresa"
- Permite agrupar variaciones morfologicas de la misma palabra

### 3.2 Parametros de Configuracion

- **Categoria gramatical**: Se procesan las palabras como sustantivos para capturar conceptos concretos
- **Longitud minima**: Palabras de al menos 2 caracteres para evitar abreviaciones sin significado
- **Idioma**: Stopwords y lematizacion configurados para español

## 4. Configuracion del Modelo LDA

### 4.1 Parametros Principales

El modelo Latent Dirichlet Allocation se configura con parametros optimizados para el analisis de respuestas gerenciales:

**Numero de Topics por Pregunta: 6**
- Equilibrio entre especificidad y generalidad
- Captura la diversidad de perspectivas sin fragmentar excesivamente
- Suficiente para identificar patrones distintos en respuestas abiertas

**Palabras Representativas por Topic: 6**
- Cantidad optima para interpretacion humana
- Suficientes palabras para comprender el tema sin ambiguedad
- Evita diluir el significado con palabras poco relevantes

**Iteraciones de Entrenamiento: 10**
- Pases completos sobre el corpus de texto
- Garantiza convergencia del modelo sin sobreajuste
- Balance entre precision y tiempo de procesamiento

**Aprendizaje Automatico de Distribuciones**
- Distribucion de topics por documento ajustada automaticamente
- Distribucion de palabras por topic optimizada durante entrenamiento
- Elimina necesidad de ajuste manual de hiperparametros

### 4.2 Filtrado del Vocabulario

Para mejorar la calidad de los topics se aplican dos criterios de filtrado:

**Palabras Poco Frecuentes**
- Se eliminan palabras que aparecen en menos de 15 documentos
- Razon: Son terminos demasiado especificos o errores de captura
- Mejora la estabilidad y generalizacion del modelo

**Palabras Muy Comunes**
- Se eliminan palabras presentes en mas del 50% de documentos
- Razon: No aportan poder discriminativo entre topics
- Ejemplos: palabras de uso general no capturadas como stopwords

## 5. Proceso de Modelado

### 5.1 Pipeline de Analisis por Pregunta

El modelado se ejecuta de forma independiente para cada una de las 6 preguntas del cuestionario, siguiendo un proceso sistematico de 7 etapas:

**Etapa 1: Seleccion de Datos**
- Se identifica la columna correspondiente a cada pregunta (P1, P2, P3, P4, P5, P6)
- Cada pregunta se trata como un corpus independiente

**Etapa 2: Aplicacion de Preprocesamiento**
- Se ejecuta la limpieza, tokenizacion y lematizacion descrita anteriormente
- Se genera una version procesada del texto para cada respuesta

**Etapa 3: Construccion del Diccionario**
- Se crea un mapeo de cada palabra unica a un identificador numerico
- Este diccionario permite representar el texto como vectores numericos

**Etapa 4: Generacion del Corpus**
- Se transforma cada documento en una representacion Bag-of-Words
- Cada respuesta se convierte en un vector de frecuencias de palabras

**Etapa 5: Entrenamiento del Modelo LDA**
- Se aplica el algoritmo con los parametros configurados
- El modelo aprende la estructura latente de topics en las respuestas

**Etapa 6: Extraccion de Topics**
- Se identifican las palabras mas representativas de cada topic
- Estas palabras caracterizan el tema subyacente

**Etapa 7: Calculo de Probabilidades**
- Para cada documento se obtiene la probabilidad de pertenencia a cada topic
- Estas probabilidades permiten clasificar y analizar las respuestas

### 5.2 Estrategia de Reutilizacion de Modelos

Para optimizar la comparabilidad entre preguntas relacionadas, se implementa una estrategia especial:

**Caso Particular: Preguntas P2 y P3**
- P2 pregunta sobre razones de renuncia en colaboradores con poco tiempo
- P3 pregunta sobre razones de renuncia en colaboradores con mucho tiempo
- Ambas abordan el mismo fenomeno (salida de personal) en diferentes contextos temporales

**Estrategia Aplicada**
- El modelo entrenado en P2 se reutiliza como base para P3
- Esto garantiza que los topics sean comparables entre ambas preguntas
- Permite identificar similitudes y diferencias en los motivos de rotacion segun antiguedad

**Beneficio**
- Mayor coherencia en la interpretacion de resultados
- Facilita analisis comparativos entre grupos de antiguedad
- Mejora la consistencia del etiquetado de topics

## 6. Estructura de Datos de Salida

### 6.1 Matriz de Probabilidades

El resultado principal del analisis es una matriz numerica que cuantifica la relacion entre respuestas y topics:

**Dimension de Filas**
- Cada fila representa una respuesta individual de un gerente
- Total de filas igual al numero de gerentes entrevistados

**Dimension de Columnas**
- Total de 36 columnas numericas
- Resultado de 6 preguntas multiplicado por 6 topics por pregunta
- Cada columna representa un topic especifico de una pregunta particular

**Valores en la Matriz**
- Probabilidades continuas entre 0 y 1
- Indican el grado de pertenencia de cada respuesta a cada topic
- Valores altos (cercanos a 1) indican fuerte presencia del topic en la respuesta
- Valores bajos (cercanos a 0) indican ausencia o irrelevancia del topic

**Nomenclatura de Columnas**
- Formato: PreguntaNumero_TipoColaborador_NumeroTopic_NombreTopic
- Ejemplo: P1_n_0_compromiso_con_la_empresa
- Permite identificacion clara del contenido de cada variable

### 6.2 Diccionario de Topics

Se mantiene un registro estructurado de la composicion de cada topic:

**Contenido del Diccionario**
- Para cada pregunta se almacenan los 6 topics identificados
- Cada topic incluye su identificador numerico y palabras clave
- Las palabras se presentan con pesos que indican su importancia relativa

**Utilidad del Diccionario**
- Permite interpretacion semantica de los topics numericos
- Facilita el etiquetado manual con nombres descriptivos
- Sirve como documentacion de la estructura latente descubierta

## 7. Interpretacion y Etiquetado de Topics

### 7.1 Topics Identificados por Pregunta

#### Pregunta 1: ¿Que crees que ha hecho que algunas personas lleven tanto tiempo en la empresa?

- Topic 0: comodidad_en_el_trabajo
- Topic 1: compromiso_con_la_empresa
- Topic 2: buen_ambiente_laboral
- Topic 3: valores_de_la_empresa
- Topic 4: estabilidad_y_crecimiento_laboral
- Topic 5: prestaciones_y_sueldo

#### Pregunta 2: ¿A que atribuyes que algunos colaboradores renuncien teniendo poco tiempo?

- Topic 0: exigencia_laboral_y_trato_jefes
- Topic 1: horarios_y_sueldo
- Topic 2: carga_de_trabajo_y_tiempo_personal
- Topic 3: busqueda_de_mejores_prestaciones
- Topic 4: disgusto_con_actividades_laborales
- Topic 5: falta_de_compromiso_y_responsabilidad

#### Pregunta 3: ¿A que atribuyes que algunos colaboradores renuncien teniendo mucho tiempo?

- Topic 0: exigencia_laboral_y_trato_jefes
- Topic 1: horarios_y_sueldo
- Topic 2: carga_de_trabajo_y_tiempo_personal
- Topic 3: busqueda_de_mejores_prestaciones
- Topic 4: disgusto_con_actividades_laborales
- Topic 5: falta_de_compromiso_o_mejores_ofertas

#### Pregunta 4: ¿Que condiciones internas/del entorno hacen que liderar este centro sea mas retador?

- Topic 0: competencia_laboral_sueldos_horarios
- Topic 1: gestion_del_centro_y_metas
- Topic 2: poca_afluencia_de_clientes
- Topic 3: ubicacion_tienda_y_competencia_zona
- Topic 4: cultura_de_trabajo_y_equipo
- Topic 5: falta_de_personal

#### Pregunta 5: ¿Que apoyos, decisiones o herramientas podrian ayudar a mejorar la permanencia?

- Topic 0: seguimiento_y_entrenamiento_colaborador
- Topic 1: apoyo_gerentes_y_desarrollo
- Topic 2: incentivos_para_metas
- Topic 3: bienestar_del_colaborador
- Topic 4: mejores_prestaciones_y_sueldos
- Topic 5: mejora_horarios_y_sueldo

#### Pregunta 6: ¿Hay algo importante que agregar sobre por que los colaboradores salen?

- Topic 0: sin_comentarios_o_tema_horarios
- Topic 1: sueldo_e_incentivos
- Topic 2: todo_bien_sin_comentarios
- Topic 3: temas_generales_colaboradores
- Topic 4: trabajo_personal_y_metas
- Topic 5: relacion_gerentes_colaboradores

## 8. Analisis Posterior

### 8.1 Ranking de Topics por Documento

Para facilitar la interpretacion se genera un ordenamiento de topics por relevancia:

**Proceso de Ranking**
- Para cada respuesta individual se ordenan los 6 topics de mayor a menor probabilidad
- Se asignan posiciones ordinales: 1ra, 2da, 3ra, 4ta, 5ta, 6ta
- Permite identificar rapidamente los temas dominantes en cada respuesta

**Utilidad del Ranking**
- Simplifica la interpretacion de respuestas complejas
- Identifica el topic principal mencionado por cada gerente
- Facilita segmentacion de gerentes por tema prioritario

### 8.2 Analisis de Co-ocurrencia de Topics

Se investigan patrones de asociacion entre topics mediante correlaciones:

**Metodo Estadistico**
- Se utiliza correlacion de Spearman para datos ordinales (rankings)
- Mide la tendencia de topics a aparecer juntos en las respuestas
- Identifica temas complementarios o relacionados

**Interpretacion**
- Correlaciones positivas altas: topics que coexisten frecuentemente
- Correlaciones negativas: topics mutuamente excluyentes
- Correlaciones cercanas a cero: topics independientes

**Aplicacion**
- Identificar clusters tematicos
- Comprender la estructura conceptual de las respuestas
- Detectar temas sistematicamente relacionados

### 8.3 Visualizaciones de Resultados

Se generan graficos especializados para comunicar hallazgos:

**Histogramas de Distribucion**
- Muestran frecuencia de cada topic en cada posicion del ranking
- Identifican topics dominantes vs secundarios
- Revelan patrones de importancia relativa

**Heatmaps de Correlacion**
- Visualizan matriz de correlaciones entre topics
- Incluyen significancia estadistica de las asociaciones
- Permiten identificacion visual de clusters tematicos

## 9. Almacenamiento de Resultados

Los resultados del modelado se persisten en tres archivos complementarios:

**Archivo 1: Matriz de Probabilidades (LDA_res.csv)**
- Formato: CSV (valores separados por comas)
- Contenido: Matriz completa de probabilidades por documento y topic
- Uso: Analisis cuantitativos posteriores, modelado predictivo, visualizaciones

**Archivo 2: Diccionario de Topics (topics_dict.pkl)**
- Formato: Pickle (serializado binario de Python)
- Contenido: Palabras clave con pesos para cada topic de cada pregunta
- Uso: Interpretacion semantica, documentacion, validacion de coherencia

**Archivo 3: Nombres de Topics (topic_names.pkl)**
- Formato: Pickle (serializado binario de Python)
- Contenido: Etiquetas descriptivas asignadas manualmente a cada topic
- Uso: Presentacion de resultados, graficos, reportes ejecutivos

**Estrategia de Almacenamiento**
- CSV para datos tabulares que requieren interoperabilidad
- Pickle para objetos Python complejos que mantienen estructura original
- Combinacion permite tanto analisis tecnico como comunicacion de resultados

## 10. Filtrado y Limpieza de Datos

### 10.1 Criterios de Inclusion

El analisis se enfoca exclusivamente en el subconjunto relevante de gerentes:

**Filtro Principal**
- Se incluyen unicamente gerentes que completaron la entrevista
- Criterio: Presencia del numero de empleado en el registro de entrevistados
- Razon: Garantizar alineacion entre datos cualitativos y cuantitativos

**Beneficio**
- Elimina ruido de respuestas parciales o fuera de alcance
- Asegura que todos los gerentes analizados tienen perfil organizacional completo
- Facilita integracion con otras fuentes de datos

### 10.2 Manejo de Respuestas Duplicadas

Se implementa una regla de deduplicacion para garantizar integridad:

**Criterio de Deduplicacion**
- Se identifica a cada gerente por su numero de empleado unico
- En caso de multiples respuestas del mismo gerente, se conserva la primera
- Se eliminan respuestas subsecuentes del mismo individuo

**Justificacion**
- Evita sobreponderacion de respuestas de gerentes que completaron la encuesta multiples veces
- Mantiene independencia estadistica de las observaciones
- Previene sesgos en analisis posteriores

**Impacto**
- Garantiza que cada gerente contribuye con peso igual al analisis
- Mejora validez estadistica de correlaciones y agrupamientos
- Simplifica interpretacion de resultados agregados

## 11. Consideraciones Metodologicas

### 11.1 Fortalezas

- **Reduccion de dimensionalidad**: Convierte texto libre en vectores numericos interpretables
- **Descubrimiento automatico**: No requiere etiquetado manual previo
- **Comparabilidad**: Topics consistentes permiten comparar patrones entre preguntas
- **Cuantificacion**: Genera metricas numericas para analisis estadistico posterior

### 11.2 Limitaciones

- **Interpretacion subjetiva**: El etiquetado de topics requiere juicio humano
- **Sensibilidad a parametros**: Resultados pueden variar con diferentes configuraciones
- **Perdida de contexto**: El modelo Bag-of-Words no captura orden de palabras
- **Dependencia del corpus**: Topics son especificos al conjunto de datos analizado

### 11.3 Buenas Practicas Implementadas

1. Filtrado de vocabulario para eliminar palabras extremas
2. Lematizacion para normalizar variantes de palabras
3. Numero balanceado de topics (ni muy pocos ni muchos)
4. Validacion manual de topics para asegurar interpretabilidad
5. Documentacion de todos los parametros y decisiones metodologicas

## 12. Uso en Analisis Posteriores

Los resultados del topic modeling se utilizan para:

1. **Analisis de sentimientos**: Comparar topics entre colaboradores actuales y salidos
2. **Correlaciones organizacionales**: Relacionar topics con variables como antiguedad, centro, region
3. **Clustering de perfiles**: Agrupar gerentes por patrones de respuesta similares
4. **Identificacion de drivers**: Detectar temas mas mencionados asociados a alta/baja rotacion
5. **Segmentacion geografica**: Comparar prevalencia de topics entre ciudades/regiones

## 13. Referencias Tecnicas

- **Algoritmo**: Latent Dirichlet Allocation (LDA)
- **Implementacion**: Gensim LdaModel
- **Lenguaje**: Python 3.x
- **Framework NLP**: NLTK para preprocesamiento
- **Idioma**: Español

---
**Documento creado**: Octubre 2025  
**Version**: 1.0  