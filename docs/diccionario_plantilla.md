
## Variables archivo plantilla original

Variables del sistema de gestion de personal y presupuesto.

| Variable | Tipo de dato | Descripcion |
| :--- | :--- | :--- |
| Fecha de inicio de vigencia | Fecha | Primer dia en que el contrato, la posicion o la asignacion entra en vigencia. |
| Fecha de cierre | Fecha | Ultimo dia previsto de vigencia del contrato o asignacion (aplicable a posiciones temporales). |
| Estado | Texto | Estado operativo del puesto o asignacion: "Activo" (ocupado/vigente) o "Inactivo" (vacante/finalizado). |
| Estado Contratacion | Texto | Estado administrativo del proceso de contratacion: "Aprobada" (autorizada) o "Congelada" (suspendida). |
| Posicion presupuestada | Texto | Indica si la posicion esta contemplada en el presupuesto ("Si") o no ("No"). |
| Importe | Numerico | Monto asignado o presupuestado para cubrir el costo anual de la posicion. |
| Indefinido o temporal | Texto | Tipo de contrato: "Indefinido" (permanente) o "Temporal" (a termino fijo). |
| Tipo de presupuesto | Texto | Clasificacion interna del gasto (ej. Operativo, Administrativo). |
| Categoria de asignacion | Texto | Motivo o naturaleza de la asignacion (ej. operacion estructural, proyecto, reemplazo). |
| Plantilla Autorizada (0/1) | Numerico/Binario | Indicador: 1 si la posicion esta en la plantilla autorizada; 0 en caso contrario. |
| Plantilla Activa (0/1) | Numerico/Binario | Indicador: 1 si la posicion esta activa o cubierta; 0 en caso contrario. |
| Vacantes (0/1) | Numerico/Binario | Indicador: 1 si existe una vacante abierta; 0 si esta cubierta o no aplica. |
| Fecha de contratacion empleador legal actual | Fecha | Fecha de inicio del contrato con la entidad legal actual. |
| Fecha de entrada en posicion actual | Fecha | Fecha en que el colaborador comenzo a desempeñar la posicion actual. |
| Tipo de Trabajador | Texto | Categoria o nivel del trabajador (ej. Colaborador, Gerente, Directivo). |
| Codigo de posicion principal | Texto | Codigo de la posicion que ejerce supervision directa sobre este puesto. |
| Nombre de posicion principal | Texto | Denominacion de la posicion supervisora directa. |
| Numero de persona del manager | Numerico | Identificador central del manager directo en el sistema de RR. HH. |
| Numero de trabajador del manager | Numerico | Identificador del contrato vigente del manager directo. |
| Nombre del manager | Texto | Nombre completo del manager directo. |

