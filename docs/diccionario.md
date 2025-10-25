# Diccionario de Variables: Analisis de Rotacion

Este documento define las variables utilizadas en el analisis de rotacion de personal, organizadas por categoria funcional.

---

## 1. Identificacion y Datos Personales del Colaborador

Variables que identifican de manera unica al colaborador y sus caracteristicas demograficas basicas.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| num_empleado | Numerico | Numero unico de identificacion del empleado en el sistema. |
| escolaridad | Texto | Nivel de estudios del colaborador (ej. Preparatoria, Licenciatura, Posgrado). |
| numero_de_persona | Numerico | Identificador central del colaborador en el sistema de RR. HH. (persistente a traves de contratos). |
| numero_de_trabajador | Numerico | Identificador de la relacion laboral o contrato vigente (puede variar con recontrataciones). |
| nombre_de_colaborador | Texto | Nombre completo de la persona asignada a la posicion. |
| antiguedad_anois | Numerico | Antiguedad del colaborador en años dentro de la organizacion. |

---

## 2. Informacion del Puesto y Rol del Manager

Variables que describen el puesto que ocupa el manager y su clasificacion organizacional.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| tipo_rol | Texto | Tipo de rol que desempeña el colaborador (ej. Manager con equipo y vacantes, Colaborador individual). |
| codigo_de_puesto | Numerico | Identificador unico del puesto segun la clasificacion organizacional. |
| nombre_de_puesto | Texto | Denominacion formal del puesto. |
| familia_de_puesto | Texto | Clasificacion amplia que agrupa puestos de naturaleza similar (ej. Ventas, Manager). |
| funcion_del_puesto | Texto | Resumen de las responsabilidades y el proposito principal del puesto. |
| codigo_de_posicion | Numerico | Identificador unico de la posicion especifica dentro del organigrama. |
| nombre_de_posicion | Texto | Denominacion de la posicion estructural (puede coincidir con el nombre de puesto). |

---

## 3. Estructura Organizacional y Ubicacion del Manager

Variables que ubican al manager dentro de la estructura organizacional y geografica de la empresa.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| centro_de_costos | Numerico | Codigo del departamento contable responsable del gasto asociado al puesto. |
| centro | Numerico | Unidad organizativa o centro de trabajo al que pertenece la posicion. |
| ubicacion | Texto | Sitio fisico o centro de trabajo donde se desempeña la posicion. |
| region | Texto | Agrupacion geografica que incluye la ubicacion del centro de trabajo. |
| ciudad | Texto | Ciudad correspondiente a la ubicacion del puesto. |
| empleador_legal | Texto | Entidad legal o razon social responsable del contrato del colaborador. |
| unidad_de_negocio | Texto | Linea o segmento de negocio al que contribuye la posicion (ej. Retail, Servicios). |
| division | Texto | Nivel jerarquico superior dentro de la unidad de negocio. |
| departamento | Texto | Area funcional especifica (ej. Operaciones, Recursos Humanos) del puesto. |
| centros_de_adscripcion_subordinados | Numerico | Numero de centros de trabajo distintos asociados a los subordinados del manager. |

---

## 4. Antiguedad y Tiempo de los Subordinados

Variables que describen las caracteristicas temporales del equipo a cargo del manager.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| fecha_antiguedad_grupo | Fecha | Fecha desde la cual se reconoce la antiguedad del colaborador en el grupo corporativo. |
| promedio_antiguedad_subordinados_anos | Numerico | Promedio de años de antiguedad del equipo a cargo del manager. |
| std_antiguedad_equipo | Numerico | Desviacion estandar de la antiguedad del equipo, mide la heterogeneidad temporal. |

---

## 5. Gestion de Personal, Equipo y Liderazgo

Variables relacionadas con la estructura del equipo y las responsabilidades de supervision del manager.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| tiene_subordinados | Booleano | Indicador de si el colaborador tiene personal a cargo (True/False). |
| lista_subordinados_trabajador | Lista | Lista de identificadores (numero_de_trabajador) de los subordinados directos. |
| lista_centros_subordinados | Lista | Lista de codigos de centros de costos de los subordinados. |
| total_indefinidos_a_cargo | Numerico | Numero de empleados con contrato indefinido (permanente) bajo supervision del manager. |
| tamano_equipo | Numerico | Numero total de personas en el equipo (subordinados directos). |

---

## 6. Variables Salariales y Compensacion de Subordinados

Variables que describen la estructura salarial del equipo a cargo del manager.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| grupos_salariales | Lista | Lista de grupos salariales de todos los subordinados directos. |
| promedio_grupo_salarial | Numerico | Promedio del grupo salarial del equipo a cargo (banda salarial media). |
| std_grupo_salarial | Numerico | Desviacion estandar del grupo salarial del equipo, mide la heterogeneidad salarial. |
| moda_grupo_salarial | Numerico | Grupo salarial mas frecuente en el equipo (banda salarial predominante). |

---

## 7. Planificacion y Presupuesto de Plantilla

Variables relacionadas con la autorizacion, presupuesto y disponibilidad de posiciones en el equipo del manager.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| total_plantilla_autorizada | Numerico | Numero maximo de empleados permitido segun autorizacion organizacional. |
| total_plantilla_activa | Numerico | Numero de empleados actualmente contratados y activos en la plantilla. |
| total_posiciones_presupuestadas | Numerico | Posiciones consideradas y aprobadas en el presupuesto organizacional. |
| posiciones_vacantes | Numerico | Posiciones autorizadas que actualmente no estan cubiertas. |
| posiciones_vacantes_activas | Numerico | Posiciones vacantes en proceso activo de reclutamiento. |
| total_contrataciones_congeladas | Numerico | Contrataciones suspendidas temporalmente por decision organizacional. |
| total_contrataciones_aprobadas | Numerico | Contrataciones que han recibido aprobacion formal para proceder. |
| indice_rigidez_contratacion | Numerico | Metrica que cuantifica la restriccion o dificultad para contratar personal (0-1). |
| pct_posiciones_vacantes | Numerico | Porcentaje de posiciones vacantes sobre el total de plantilla autorizada. |

---

## 8. Metricas de Rotacion y Movimiento de Personal (Multicentro)

Variables que cuantifican la rotacion de personal considerando todos los centros bajo supervision del manager.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| planta_mensual_multicentro | Numerico | Numero promedio de empleados activos en el mes, sumando todos los centros a cargo. |
| bajas_mensual_multicentro | Numerico | Numero total de salidas o bajas en el mes, agregando todos los centros a cargo. |
| rotacion_mensual_pct_multicentro | Numerico | Tasa de rotacion mensual en porcentaje, calculada sobre la planta multicentro. |
| planta_promedio_anual_movil_multicentro | Numerico | Promedio de empleados activos en los ultimos 12 meses, considerando todos los centros. |
| bajas_anuales_moviles_multicentro | Numerico | Total de salidas o bajas en los ultimos 12 meses (anual movil), multicentro. |
| rotacion_anual_movil_pct_multicentro | Numerico | Tasa de rotacion anual movil en porcentaje, calculada sobre la planta promedio multicentro. |

---

**Nota**: Las variables estan organizadas en 8 categorias principales que facilitan el analisis de rotacion desde multiples perspectivas: individual, organizacional, temporal, salarial, presupuestal y operativa.
