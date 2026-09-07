# CASOS DE PRUEBA — MIRA

Derivados **estrictamente** de la tabla de Requisitos Funcionales y No Funcionales (`requisitosFuncionales_y_NoFuncionales.md`). Cada caso de prueba se traza a uno o más requisitos (RF-XX / RNF-XX) y no introduce reglas, umbrales ni comportamientos que no aparezcan en el texto de ese requisito o en su columna **Verificación**.

## Convenciones

- **ID:** `CP-RF-XX-nn` para casos de prueba funcionales; `CP-RNF-XX-nn` para casos de prueba extra-funcionales (no funcionales).
- **Tipo:** *Funcional* / *Extra-funcional (No Funcional)*. Se conserva la subcategoría del RNF de origen (Rendimiento, Confiabilidad, Auditabilidad, etc.).
- **Prioridad:** heredada del requisito de origen, en escala **MoSCoW** (Must / Should / Could).
- **Criterio de aceptación:** cuando el requisito tiene un criterio medible en su columna **Verificación**, se transcribe. Cuando esa columna es `null`, se indica explícitamente `Verificación = null en el requisito` y el criterio se limita a la verificación cualitativa del texto del requisito, sin inventar un umbral.
- Casos negativos / de flujo alternativo se incluyen **solo** cuando el propio texto del requisito enuncia una prohibición, una condición binaria o un porcentaje objetivo (verbos "no debe", "nunca", "únicamente", "0%", "100%").

---

## 1. Casos de Prueba Funcionales (CP-RF)

### RF-01 — Tres canales de ingesta de evidencia

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-01-01 | Ingresar evidencia vía API | La organización tiene el canal API habilitado | 1) Enviar una pieza de evidencia legible por la API. 2) Observar la respuesta. | La evidencia se acepta y queda asociada a un caso (nuevo o existente). | Verificación = null en el requisito. Los tres canales (API, carga manual, casilla/carpeta) deben estar disponibles y aceptar evidencia. | Must |
| CP-RF-01-02 | Ingresar evidencia por carga manual desde el navegador | Usuario autenticado con acceso a carga manual | 1) Cargar un archivo de evidencia desde la interfaz web. 2) Confirmar el envío. | La evidencia se acepta y queda asociada a un caso. | Ídem CP-RF-01-01. | Must |
| CP-RF-01-03 | Ingresar evidencia por recolección periódica desde casilla/carpeta | Existe una casilla o carpeta configurada como canal de ingesta | 1) Depositar un archivo de evidencia en la casilla/carpeta. 2) Esperar el ciclo de revisión periódica. | La plataforma recoge el archivo y lo asocia a un caso. | Ídem CP-RF-01-01. | Must |

### RF-02 — Verificación de legibilidad antes del análisis

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-02-01 | Verificar evidencia legible antes de analizar | Canal de ingesta habilitado | 1) Ingresar una evidencia legible y utilizable. 2) Consultar el rastro del caso. | El sistema marca la evidencia como verificada y recién entonces la envía a los módulos de análisis. | Verificación = null. La verificación de legibilidad precede a cualquier ejecución de módulo. | Must |
| CP-RF-02-02 | La verificación ocurre antes del análisis | Canal de ingesta habilitado | 1) Ingresar una evidencia no utilizable. 2) Revisar si se ejecutó algún módulo de análisis sobre ella. | Ningún módulo de análisis se ejecuta sobre evidencia que no pasó la verificación de legibilidad. | Ídem. | Must |

### RF-03 — Rechazo con razón comprensible

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-03-01 | Rechazar archivo no legible informando la razón | Canal de ingesta habilitado | 1) Ingresar un archivo corrupto o ilegible. 2) Observar la respuesta/notificación. | El sistema rechaza el archivo y comunica la razón en lenguaje comprensible para una persona no técnica; no queda en procesamiento silencioso ni falla sin aviso. | Verificación = null (no se define cómo medir "comprensible"). El rechazo es explícito y acompañado de una razón legible. | Should |

### RF-04 — Identificador de caso único y permanente

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-04-01 | Crear caso con identificador único ante la primera evidencia | No existe caso previo sobre el hecho | 1) Ingresar la primera evidencia sobre un hecho. | Se crea un caso con un identificador único y permanente. | El identificador debe permanecer idéntico entre la creación y cualquier reevaluación posterior del mismo caso. | Must |
| CP-RF-04-02 | Asociar evidencia adicional al mismo caso abierto | Existe un caso abierto | 1) Ingresar una segunda evidencia sobre el mismo hecho. | La evidencia se asocia al caso existente; el identificador no cambia. | Ídem. | Must |
| CP-RF-04-03 | Identificador estable tras reevaluación | Caso abierto con conclusión previa | 1) Ingresar evidencia adicional que dispara reevaluación (ver RF-13/UC-13). 2) Comparar el identificador antes y después. | El identificador del caso es idéntico antes y después de la reevaluación. | Ídem (criterio explícito del requisito). | Must |

### RF-05 — Caso nuevo vinculado ante evidencia sobre caso cerrado

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-05-01 | Evidencia nueva sobre caso cerrado abre caso vinculado | Existe un caso en estado CERRADO | 1) Ingresar evidencia sobre el mismo hecho del caso cerrado. 2) Revisar el estado del caso original y el nuevo caso. | El caso CERRADO no cambia de estado; se genera un caso nuevo con identificador distinto y referencia al original. | Un caso en estado CERRADO no debe cambiar de estado al recibir evidencia nueva; debe generarse un identificador de caso distinto con referencia al original. | Must |

### RF-06 — Registro de eventos en el rastro de auditoría

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-06-01 | Registrar ingreso de evidencia, ejecución de módulos y decisión | Caso procesado de extremo a extremo | 1) Procesar un caso completo (ingreso de evidencia → módulos → decisión). 2) Consultar el rastro de auditoría del caso. | El rastro contiene, como mínimo, entradas para el ingreso de evidencia, la ejecución de módulos y la decisión. | Verificación = null. Cada uno de los eventos relevantes citados en el requisito produce una entrada de auditoría. | Must |

### RF-07 — Ejecución de módulos activos del flujo vigente

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-07-01 | Ejecutar los módulos activos del flujo vigente | Caso con evidencia legible; flujo vigente con módulos activos | 1) Procesar el caso. 2) Comparar los módulos ejecutados contra los módulos activos del flujo vigente. | Se ejecutan exactamente los módulos activos del flujo vigente sobre la evidencia. | Verificación = null. Correspondencia entre módulos ejecutados y módulos activos del flujo vigente. | Must |
| CP-RF-07-02 | No ejecutar módulos inactivos | Flujo con al menos un módulo inactivo/deprecado | 1) Procesar un caso con ese flujo. 2) Revisar los módulos ejecutados. | Los módulos no activos del flujo no se ejecutan. | Ídem. | Must |

### RF-08 — Puntaje de confianza 0-100 con desglose

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-08-01 | Puntaje dentro de rango | Caso con evidencia legible y módulos activos | 1) Procesar el caso. 2) Leer el valor del puntaje. | El puntaje está dentro del rango [0,100]. | El puntaje debe estar dentro del rango [0,100] en el 100% de los casos con puntaje calculado. | Must |
| CP-RF-08-02 | Desglose de componentes no vacío | Caso con puntaje calculado | 1) Procesar varios casos. 2) Verificar que cada puntaje incluye su desglose de señales. | Todo caso con puntaje calculado presenta un desglose de componentes no vacío. | El desglose de componentes no debe estar vacío en el 100% de los casos con puntaje calculado. | Must |

### RF-09 — Comparación del puntaje contra los umbrales

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-09-01 | Comparar puntaje contra umbrales configurados | Existe configuración de umbrales (aprobación, derivación, escalamiento) | 1) Procesar casos con puntajes que caen en cada rango. 2) Observar la decisión resultante. | La decisión (aprobación / derivación / escalamiento) corresponde al rango de umbral en que cae el puntaje. | Verificación = null. La decisión es coherente con el rango de umbral del puntaje. | Must |

### RF-10 — Aprobación automática solo con puntaje alto y sin fraude

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-10-01 | Aprobar automáticamente con puntaje ≥ umbral y sin fraude | Umbral de aprobación configurado | 1) Procesar un caso cuyo puntaje supera el umbral de aprobación y sin señal de fraude activa. | El caso se aprueba automáticamente. | Aprobación automática solo cuando el puntaje supera el umbral de aprobación y no existe ninguna señal de fraude activa. | Must |
| CP-RF-10-02 | No aprobar automáticamente si hay señal de fraude activa | Umbral de aprobación configurado | 1) Procesar un caso con puntaje ≥ umbral de aprobación **y** una señal de fraude activa. | El caso **no** se aprueba automáticamente (se deriva o escala, ver RF-12). | 0% de casos con decisión de aprobación automática que tengan una señal de fraude activa asociada. | Must |
| CP-RF-10-03 | No aprobar automáticamente bajo el umbral de aprobación | Umbral de aprobación configurado | 1) Procesar un caso con puntaje por debajo del umbral de aprobación. | El caso no se aprueba automáticamente. | Ídem criterio del requisito (condición necesaria: puntaje > umbral de aprobación). | Must |

### RF-11 — Derivación por rango intermedio o señal bajo su mínimo

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-11-01 | Derivar caso con puntaje en rango intermedio | Umbrales configurados | 1) Procesar un caso cuyo puntaje cae en el rango de derivación. | El caso se deriva a revisión asistida. | Verificación = null (los mínimos por señal no están definidos en el texto). | Must |
| CP-RF-11-02 | Derivar caso con señal individual bajo su mínimo | Existe un mínimo por señal definido (parámetro de configuración) | 1) Procesar un caso con puntaje total suficiente pero una señal individual bajo su mínimo. | El caso se deriva a revisión asistida pese al puntaje total. | Verificación = null: caso parametrizable, no ejecutable con un umbral fijo hasta que se definan los mínimos por señal. | Must |

### RF-12 — Derivar o escalar ante señal de fraude activa

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-12-01 | Fraude activo con puntaje alto se deriva/escala | Umbral de aprobación configurado | 1) Procesar un caso con señal de fraude activa y puntaje ≥ umbral de aprobación. | El caso se deriva o escala; nunca se aprueba automáticamente. | 0% de casos con señal de fraude activa resueltos con decisión de aprobación automática. | Must |

### RF-13 — Derivación ante módulo caído (sin aprobación parcial)

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-13-01 | Módulo en error deriva el caso indicando qué faltó | Flujo con varios módulos activos | 1) Forzar el fallo/indisponibilidad de un módulo del flujo. 2) Procesar un caso. | El caso se deriva a revisión, con indicación del módulo o información que faltó; no se aprueba con información parcial. | 0% de casos con un módulo en estado de error resueltos con decisión de aprobación automática. | Must |

### RF-14 — Prohibición del rechazo automático

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-14-01 | Puntaje muy bajo escala o deriva, no rechaza | Umbral de escalamiento configurado | 1) Procesar un caso con puntaje por debajo del umbral inferior. | El caso se escala o deriva a revisión humana; no se rechaza automáticamente. | El sistema no debe permitir el rechazo automático de un caso. | Must |
| CP-RF-14-02 | Todo rechazo tiene autor humano | Existen casos con resultado "rechazado" | 1) Consultar todas las decisiones con resultado "rechazado". 2) Verificar el autor responsable. | Toda decisión "rechazado" tiene registrado un operador humano como autor. | 0% de decisiones con resultado "rechazado" sin un autor humano (operador) registrado. | Must |

### RF-15 — Pantalla única de revisión asistida

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-15-01 | Evidencia, señales y razón de derivación en una sola pantalla | Caso derivado en la bandeja del operador | 1) Abrir el caso derivado en la bandeja de revisión asistida. | La pantalla muestra simultáneamente la evidencia, el desglose de señales y la razón de derivación, sin navegación adicional. | Verificación = null. Los tres elementos aparecen en una única pantalla. | Must |

### RF-16 — Ver el hallazgo de la plataforma en cada pieza de evidencia

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-16-01 | Abrir una pieza y ver qué encontró la plataforma y dónde | Caso derivado en revisión | 1) Abrir una pieza de evidencia dentro de la revisión asistida. | Se indica/resalta qué encontró el módulo y en qué parte del documento o imagen. | Verificación = null. | Should |

### RF-17 — Puntaje mostrado al final de la pantalla de revisión

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-17-01 | El puntaje no es el primer elemento visible | Caso derivado en revisión | 1) Abrir la pantalla de revisión asistida. 2) Observar el orden de los elementos. | El componente de puntaje aparece después de la evidencia y las alertas; no es el primer elemento visible. | El componente de puntaje no debe ser el primer elemento visible al abrir la pantalla de revisión asistida. | Should |

### RF-18 — El operador aprueba, rechaza o solicita antecedentes

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-18-01 | Aprobar caso derivado con motivo | Operador con caso en revisión | 1) Seleccionar "aprobar". 2) Ingresar el motivo. 3) Confirmar. | La decisión de aprobación y su motivo quedan registrados. | Verificación = null. Se registran decisión y motivo. | Must |
| CP-RF-18-02 | Rechazar caso derivado con motivo | Operador con caso en revisión | 1) Seleccionar "rechazar". 2) Ingresar el motivo. 3) Confirmar. | La decisión de rechazo y su motivo quedan registrados, con el operador como autor. | Ídem; se relaciona con RF-14-02. | Must |
| CP-RF-18-03 | Solicitar antecedentes adicionales | Operador con caso en revisión | 1) Seleccionar "solicitar antecedentes". 2) Ingresar el motivo. 3) Confirmar. | La solicitud y su motivo quedan registrados. | Ídem. | Must |

### RF-19 — Marcado de discrepancia operador vs. plataforma

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-19-01 | Decisión distinta de la sugerencia queda marcada | Caso en revisión con una sugerencia de la plataforma | 1) El operador decide de forma distinta a la sugerencia. 2) Confirmar. 3) Revisar el estado del caso. | El caso queda marcado para revisión del equipo de modelos; la decisión del operador se mantiene sin alterar. | 100% de los casos con decisión del operador distinta de la sugerencia deben tener el indicador de discrepancia registrado. | Should |
| CP-RF-19-02 | Decisión igual a la sugerencia no se marca | Caso en revisión con sugerencia de la plataforma | 1) El operador decide igual que la sugerencia. 2) Confirmar. | El caso no se marca como discrepancia. | Ídem (condición complementaria del criterio). | Should |

### RF-20 — Alerta por plazo próximo a vencer

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-20-01 | Alertar al operador cuando el caso se acerca al vencimiento | Caso derivado con plazo comprometido | 1) Hacer avanzar el tiempo hasta que el caso esté próximo a vencer. 2) Revisar las alertas del operador. | El operador recibe una alerta de plazo por vencer. | Verificación = null (no se especifica la anticipación). Existe una alerta antes del vencimiento. | Should |

### RF-21 — Creación de flujo sin código

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-21-01 | Armar un flujo conectando bloques sin escribir código | Usuario con rol Diseñador de Flujo | 1) Arrastrar y conectar bloques de ingesta, análisis, cálculo de puntaje, reglas, decisión e integraciones. 2) Configurar cada bloque desde la interfaz. 3) Guardar. | El flujo se crea y se guarda sin requerir escritura de código. | Verificación = null. | Must |

### RF-22 — Prueba de flujo con resultado paso a paso

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-22-01 | Probar el flujo sobre casos de ejemplo antes de publicar | Flujo en borrador; casos de ejemplo disponibles | 1) Ejecutar la prueba del flujo sobre casos de ejemplo. 2) Observar la salida. | El sistema muestra el resultado del flujo paso a paso por bloque, sin publicarlo. | Verificación = null. | Must |

### RF-23 — No modificar flujo publicado

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-23-01 | Intento de edición directa sobre flujo publicado | Existe un flujo en estado PUBLICADO | 1) Intentar editar directamente el flujo publicado. | El sistema rechaza la edición directa o fuerza la creación de una nueva versión (en borrador). | Un intento de edición directa sobre un flujo en estado publicado debe ser rechazado por el sistema o forzar la creación de una nueva versión. | Must |

### RF-24 — Casos en curso terminan con su versión inicial

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-24-01 | Publicar versión nueva mientras hay casos en curso | Hay casos en estado PROCESANDO con la versión N del flujo | 1) Publicar la versión N+1 del flujo. 2) Dejar terminar los casos en curso. 3) Procesar un caso nuevo. | Los casos en curso se completan con la versión N; los casos nuevos usan la versión N+1. | Verificación = null. | Must |

### RF-25 — No publicar flujo sin módulo de análisis activo

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-25-01 | Rechazar publicación de flujo con 0 módulos activos | Flujo en borrador sin módulos de análisis activos | 1) Intentar publicar el flujo. | La publicación es rechazada. | Intento de publicación de un flujo con 0 módulos activos debe ser rechazado. | Should |
| CP-RF-25-02 | Permitir publicación de flujo con ≥1 módulo activo | Flujo en borrador con al menos un módulo de análisis activo | 1) Publicar el flujo. | La publicación se completa. | Ídem (condición complementaria). | Should |

### RF-26 — Revertir flujo a versión anterior

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-26-01 | Revertir a una versión anterior del flujo | Existe más de una versión del flujo | 1) Seleccionar una versión anterior. 2) Revertir. 3) Procesar un caso nuevo. | La versión anterior queda vigente para casos nuevos. | Verificación = null. | Should |

### RF-27 — Proponer un nuevo valor de umbral

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-27-01 | Diseñador de Flujo propone un umbral | Usuario con rol Diseñador de Flujo | 1) Proponer un nuevo valor para el umbral de aprobación / derivación / escalamiento. | La propuesta queda registrada como pendiente de aprobación. | Verificación = null. | Must |
| CP-RF-27-02 | Usuario sin rol Diseñador de Flujo no puede proponer | Usuario con otro rol | 1) Intentar proponer un cambio de umbral. | La acción es rechazada. | Ídem: el requisito atribuye la acción al rol Diseñador de Flujo. | Must |

### RF-28 — Segunda aprobación (Compliance) para cambio de umbral

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-28-01 | Cambio sin segunda aprobación no entra en vigencia | Propuesta de cambio de umbral registrada | 1) Dejar la propuesta sin aprobación de Compliance. 2) Procesar casos. | El umbral anterior sigue vigente; el cambio no se aplica. | 0% de cambios de umbral vigentes sin un segundo registro de aprobación distinto del solicitante. | Must |
| CP-RF-28-02 | Cambio con aprobación de Compliance entra en vigencia | Propuesta registrada + usuario Compliance distinto del solicitante | 1) Aprobar el cambio como Compliance. 2) Procesar casos posteriores. | El nuevo umbral queda vigente para decisiones posteriores. | Ídem (condición complementaria). | Must |
| CP-RF-28-03 | El aprobador no puede ser el solicitante | Propuesta registrada por el usuario X | 1) Intentar que el usuario X apruebe su propia propuesta. | La aprobación es rechazada. | "un segundo registro de aprobación distinto del solicitante". | Must |

### RF-29 — Registro del cambio de umbral

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-29-01 | Registrar autor, fecha y valor anterior del cambio | Cambio de umbral efectuado | 1) Realizar un cambio de umbral. 2) Consultar su registro. | El registro contiene autor, fecha y valor anterior, todos no nulos. | 100% de los registros de cambio de umbral deben contener autor, fecha y valor anterior no nulos. | Must |

### RF-30 — Reconstrucción de la decisión de un caso

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-30-01 | Reconstruir la decisión con las versiones vigentes en su momento | Caso decidido; luego se actualizan módulos y/o flujo | 1) Actualizar la versión de módulos y del flujo. 2) Reconstruir la decisión del caso anterior. | La reconstrucción usa las versiones de módulo y flujo vigentes al momento de decidir, no las actuales. | Verificación = null (criterio cuantitativo en RNF-04). | Must |

### RF-31 — Exportación del rastro de auditoría

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-31-01 | Exportar el rastro completo de un caso | Caso con rastro de auditoría registrado; actor autorizado | 1) Solicitar la exportación del rastro de auditoría del caso. 2) Abrir el archivo exportado. | El archivo contiene evidencia, módulos ejecutados y su versión, reglas aplicadas, puntaje, decisión y quién intervino, en un formato entregable a un tercero. | Verificación = null (no se especifica el formato exacto). Todos los elementos enumerados están presentes. | Must |

### RF-32 — Restricción de acceso de DPRIME a evidencia

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-32-01 | Acceso de DPRIME sin autorización registrada es denegado | Personal DPRIME sin autorización sobre el caso | 1) Personal DPRIME intenta acceder a la evidencia del caso. | El acceso es denegado y queda registrado como acceso denegado. | 0% de accesos de personal DPRIME a evidencia de cliente sin un registro de autorización previo asociado. | Must |
| CP-RF-32-02 | Acceso de DPRIME con autorización expresa registrada es permitido | Existe autorización expresa y registrada del cliente para atender un incidente | 1) Personal DPRIME accede a la evidencia del caso autorizado. | El acceso se concede y queda registrado. | Ídem (condición complementaria). Se relaciona con RF-47. | Must |

### RF-33 — Panel de indicadores de operación

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-33-01 | El panel muestra los siete indicadores | Existen casos procesados en el período consultado | 1) Abrir el panel de indicadores de la organización. | El panel muestra: volumen de casos por período y flujo, proporción resuelta automáticamente, proporción derivada, tiempo promedio de resolución, distribución de puntajes, tasa de detección de fraude y casos en que el operador contradijo a la plataforma. | Verificación = null. Los siete indicadores enumerados están presentes. | Should |

### RF-34 — Corrección de un dato mal leído

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-34-01 | El operador marca y corrige un dato mal leído | Operador revisando un caso con un dato mal leído | 1) Marcar el dato como incorrecto. 2) Ingresar el valor correcto. 3) Confirmar. | La corrección se acepta y queda asociada al caso. | Verificación = null. | Should |

### RF-35 — La corrección no sobrescribe el valor original

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-35-01 | El valor original se conserva junto al corregido | Dato corregido por el operador | 1) Corregir un dato. 2) Consultar el registro del dato. | El valor original permanece disponible junto al valor corregido; no se sobrescribe ni se elimina. | 0% de correcciones que eliminen o sobrescriban el valor original del registro. | Should |

### RF-36 — Comparación de versión nueva de módulo

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-36-01 | Comparar versión nueva vs. vigente sobre casos resueltos | Existe versión nueva de un módulo y un conjunto de casos ya resueltos | 1) Ejecutar la comparación de la versión nueva contra la vigente sobre el conjunto de casos resueltos. | El sistema produce un reporte comparativo del comportamiento de ambas versiones, sin activar la versión nueva. | Verificación = null. | Should |

### RF-37 — Conteo de casos con decisión distinta entre versiones

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-37-01 | Mostrar en cuántos casos la decisión habría cambiado | Comparación de versiones ejecutada (CP-RF-36-01) | 1) Consultar el resultado de la comparación. | El resultado incluye un conteo numérico de casos con decisión distinta entre ambas versiones. | El sistema debe entregar un conteo numérico de casos con decisión distinta entre ambas versiones. | Could |

### RF-38 — Actualización de módulo sin alterar producción sin autorización

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-38-01 | Activación de versión nueva sin autorización del cliente no se aplica | Flujo en producción; versión nueva de módulo disponible; sin autorización del cliente | 1) Intentar activar la versión nueva en el flujo de producción. | La versión nueva no se aplica al flujo de producción. | 0% de flujos en producción con cambio de versión de módulo sin un registro de autorización del cliente. | Must |
| CP-RF-38-02 | Activación con autorización registrada del cliente se aplica | Existe autorización explícita registrada del cliente | 1) Activar la versión nueva en el flujo de producción. 2) Consultar el registro. | La versión nueva se aplica y queda el registro de autorización asociado. | Ídem (condición complementaria). | Must |

### RF-39 — Conteo de validaciones en tiempo real

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-39-01 | El conteo coincide entre la vista del cliente y la de DPRIME | Existen validaciones registradas para un cliente | 1) Consultar el contador de validaciones desde la vista del cliente y desde la vista DPRIME en el mismo instante. | Ambos valores coinciden. | El número de validaciones mostrado al cliente y a DPRIME debe coincidir (diferencia = 0) en cualquier momento de consulta. | Must |

### RF-40 — Detalle de consumo al cierre del período

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-40-01 | Generar el detalle de consumo al cierre | Existen casos procesados en el período | 1) Ejecutar el cierre del período. 2) Consultar el detalle generado. | El sistema genera el detalle de consumo necesario para emitir la factura. | Verificación = null. | Must |

### RF-41 — Exclusión de casos no cobrables

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-41-01 | Caso con evidencia ilegible no se contabiliza | Caso rechazado por evidencia ilegible | 1) Generar un caso con evidencia ilegible. 2) Consultar el conteo de validaciones facturables. | El caso no aparece en el conteo de validaciones facturables. | 0% de casos no procesables incluidos en el conteo de validaciones facturables. | Must |
| CP-RF-41-02 | Reintento por error interno no incrementa el conteo | Caso reprocesado por un error interno de la plataforma | 1) Forzar un reintento por error interno. 2) Consultar el conteo. | El reintento no incrementa el conteo de validaciones facturables. | 0% de reintentos internos incluidos en el conteo de validaciones facturables. | Must |

### RF-42 — Autenticación contra repositorio propio

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-42-01 | Login con credenciales válidas | Usuario con cuenta activa propia | 1) Ingresar credenciales válidas. | Acceso concedido con los permisos correspondientes al rol del usuario. | Verificación = null. | Must |
| CP-RF-42-02 | Login con credenciales inválidas | Usuario con cuenta activa propia | 1) Ingresar credenciales inválidas. | Acceso denegado. | Ídem (comportamiento implícito en "validar las credenciales antes de conceder acceso"). | Must |

### RF-43 — Autenticación federada

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-43-01 | Validar contra el directorio corporativo cuando la organización lo requiere | Organización configurada con autenticación federada | 1) Un usuario de esa organización inicia sesión. | Las credenciales se validan contra el directorio corporativo del cliente, no contra el repositorio propio. | Verificación = null. | Could |

### RF-44 — Ejercicio del derecho de eliminación

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-44-01 | Eliminación sin obligación legal que prevalezca | Solicitud de eliminación de un titular; sin obligación legal aplicable | 1) Registrar la solicitud. 2) El sistema evalúa la existencia de obligación legal. 3) Procesar la eliminación. | El sistema elimina los datos personales solicitados. | Verificación = null (criterio cuantitativo en RNF-09). | Must |
| CP-RF-44-02 | Eliminación con obligación legal que prevalezca | Solicitud de eliminación; existe obligación legal de conservación | 1) Registrar la solicitud. 2) El sistema evalúa la obligación legal. | Los datos se conservan; no se eliminan. | Ídem; se relaciona con RF-45. | Must |

### RF-45 — Registro auditable de la resolución de la solicitud

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-45-01 | Registrar la razón de conservación o eliminación | Solicitud de eliminación resuelta (eliminada o conservada) | 1) Resolver una solicitud de eliminación. 2) Consultar su registro. | Existe un registro auditable con la razón (eliminado o conservado), no vacío. | 100% de las solicitudes de eliminación deben tener un registro de razón (eliminado o conservado) no vacío. | Must |

### RF-46 — No eliminar evidencia con caso abierto dependiente

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-46-01 | Evidencia ligada a caso no cerrado no se elimina | Evidencia asociada a un caso en estado ABIERTO / PROCESANDO / DERIVADO | 1) Solicitar la eliminación de esa evidencia. | La evidencia no se elimina mientras el caso no esté cerrado. | 0% de evidencias eliminadas mientras su caso asociado esté en un estado distinto de cerrado. | Must |
| CP-RF-46-02 | Evidencia ligada a caso cerrado puede eliminarse | Evidencia asociada únicamente a casos en estado CERRADO; sin obligación legal de conservación | 1) Solicitar la eliminación. | La eliminación procede (sujeta a RF-44 / RF-46). | Ídem (condición complementaria). | Must |

### RF-47 — Registro de cada acceso de DPRIME a evidencia

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-47-01 | Registrar quién, cuándo y con qué autorización | Acceso autorizado de personal DPRIME a evidencia de un cliente | 1) Personal DPRIME accede a la evidencia autorizado. 2) Consultar el registro de auditoría. | El registro contiene el actor, la fecha/hora y la referencia a la autorización. | 100% de los accesos de personal DPRIME deben quedar registrados con actor, fecha/hora y referencia a la autorización. | Must |

### RF-48 — Detección de eventos y enrutamiento de notificaciones

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-48-01 | Evento de plazo por vencer se notifica al operador | Caso derivado próximo a vencer | 1) Disparar el evento "plazo por vencer". | La notificación llega al operador. | Verificación = null. El destinatario corresponde al tipo de evento. | Should |
| CP-RF-48-02 | Evento de bandeja anómala se notifica al supervisor | Se detecta una bandeja anómala | 1) Disparar el evento "bandeja anómala". | La notificación llega al supervisor. | Ídem. | Should |
| CP-RF-48-03 | Evento de caída de servicio externo se notifica a DPRIME | Un servicio externo deja de responder | 1) Disparar el evento "caída de servicio externo". | La notificación llega al equipo de DPRIME. | Ídem. | Should |

### RF-49 — Doble canal de notificación (correo e interfaz)

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-49-01 | Notificación por correo y en la interfaz | Se genera un evento que requiere atención humana | 1) Disparar el evento. 2) Revisar el correo del destinatario y su interfaz. | La notificación se envía por correo electrónico y también se muestra en la interfaz. | Verificación = null. Ambos canales reciben la notificación. | Should |

### RF-50 — Callback al sistema de origen

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-50-01 | Callback cuando el cliente lo habilitó | Cliente con callback configurado hacia una dirección propia | 1) Generar un evento notificable para un caso de ese cliente. | El sistema realiza la llamada de vuelta a la dirección configurada por el cliente. | Verificación = null. | Could |
| CP-RF-50-02 | Sin callback configurado no hay llamada externa | Cliente sin callback configurado | 1) Generar el mismo evento. | No se realiza ninguna llamada externa. | Ídem ("cuando este lo haya habilitado"). | Could |

### RF-51 — Plantillas preconfiguradas por caso de uso

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-51-01 | Incorporar un cliente nuevo desde una plantilla de industria | Existe una plantilla preconfigurada para el caso de uso del cliente | 1) Crear el cliente nuevo a partir de la plantilla. | Se cargan los módulos, reglas y umbrales sugeridos de la plantilla. | Verificación = null. | Should |

### RF-52 — Tiempos de incorporación de cliente nuevo

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-52-01 | Caso de prueba el mismo día de la firma | Cliente firmado; plantilla disponible | 1) Registrar la fecha de firma. 2) Procesar el primer caso de prueba. 3) Medir la diferencia. | El primer caso de prueba se procesa a ≤ 1 día de la firma. | Tiempo entre firma y primer caso de prueba ≤ 1 día. | Could |
| CP-RF-52-02 | Producción dentro de dos semanas | Cliente firmado | 1) Registrar la fecha de firma. 2) Registrar la fecha de paso a producción. 3) Medir la diferencia. | El paso a producción ocurre a ≤ 14 días de la firma. | Tiempo entre firma y operación en producción ≤ 14 días. | Could |

### RF-53 — Reintento ante caída de servicio externo sin descartar casos

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-53-01 | Servicio externo caído: reintento con criterio, sin descartes | El motor depende de un servicio externo; se simula su caída | 1) Simular la caída del servicio externo. 2) Enviar casos nuevos durante la caída. 3) Restablecer el servicio. | El sistema reintenta con criterio en lugar de fallar de inmediato, mantiene los casos en cola y no descarta ninguno; al restablecerse, los procesa. | 0% de casos descartados por caída de un servicio externo. | Must |

### RF-54 — Aviso de procesamiento demorado

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-54-01 | Avisar a DPRIME y al cliente afectado | Procesamiento demorado por caída de un servicio externo | 1) Simular la demora. 2) Revisar las notificaciones. | Tanto DPRIME como el cliente afectado reciben aviso del procesamiento demorado. | Verificación = null. Ambos destinatarios reciben el aviso. | Should |

### RF-55 — Encolamiento de llamadas sobre el límite

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RF-55-01 | Llamadas excedentes se encolan con respuesta de posición | Límite de llamadas por minuto configurado para el ambiente | 1) Enviar llamadas a la API por encima del límite configurado. 2) Observar las respuestas. | Las llamadas excedentes se encolan; cada una recibe de inmediato una respuesta con su posición en la cola; ninguna es rechazada. | 0% de llamadas rechazadas por exceso de límite; 100% de las llamadas excedentes reciben una respuesta de encolamiento. | Must |

---

## 2. Casos de Prueba Extra-funcionales (CP-RNF)

### RNF-01 — Rendimiento / Escalabilidad de la API

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-01-01 | Soportar 100 llamadas/minuto en integración sin rechazos | Ambiente de integración disponible | 1) Ejecutar una prueba de carga sostenida de 100 llamadas por minuto contra la API de integración. | Ninguna llamada es rechazada. | Prueba de carga: pico de 100 llamadas por minuto (integración) sin llamadas rechazadas. | Must |
| CP-RNF-01-02 | Soportar 1000 llamadas/minuto en producción sin rechazos | Ambiente de producción (o equivalente de prueba) disponible | 1) Ejecutar una prueba de carga sostenida de 1000 llamadas por minuto contra la API de producción. | Ninguna llamada es rechazada. | Prueba de carga: pico de 1000 llamadas por minuto (producción) sin llamadas rechazadas. | Must |
| CP-RNF-01-03 | El excedente se encola, no se rechaza | API con límite configurado | 1) Superar el límite del ambiente con un pico de llamadas. | El excedente se encola; 0 llamadas rechazadas. | "sin rechazar el excedente" (consistente con RF-55). | Must |

### RNF-02 — Confiabilidad ante caída de servicio externo

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-02-01 | Cero casos perdidos en caída simulada | Sistema dependiente de un servicio externo | 1) Simular la caída del servicio externo. 2) Enviar un lote conocido de casos nuevos durante la caída. 3) Restablecer el servicio. 4) Contar los casos procesados. | Se sigue aceptando casos nuevos durante la caída; al restablecerse, se procesan todos; 0 casos perdidos. | 0 casos perdidos en una prueba de caída simulada de un servicio externo. | Must |

### RNF-03 — Eficiencia operativa de incorporación

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-03-01 | Incorporación de cliente ≤ 14 días corridos | Cliente firmado; plantilla de industria disponible | 1) Registrar la fecha de firma. 2) Verificar la operación de prueba el mismo día. 3) Registrar la fecha de paso a producción. 4) Medir el intervalo firma→producción. | Operación de prueba el día de la firma y paso a producción a ≤ 14 días corridos de la firma. | Tiempo de incorporación ≤ 14 días corridos desde la firma. | Could |

### RNF-04 — Auditabilidad: reconstrucción exacta de decisiones

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-04-01 | Reconsulta sin discrepancias de configuración | Conjunto de casos decididos; posteriores cambios de versión de módulo y de flujo | 1) Registrar la configuración vigente al momento de decidir cada caso. 2) Cambiar versiones de módulo y flujo. 3) Reconsultar cada caso. 4) Comparar la configuración mostrada con la registrada. | La configuración mostrada al reconsultar coincide con la registrada al momento de decidir. | 0 discrepancias entre la configuración registrada al momento de decidir y la mostrada al reconsultar el caso. | Must |

### RNF-05 — Confidencialidad / Control de acceso a evidencia

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-05-01 | Usuario de otra organización no accede a la evidencia | Evidencia de la organización A; usuario de la organización B | 1) El usuario B intenta acceder a la evidencia de un caso de A. | Acceso denegado. | 0% de accesos a evidencia de un cliente realizados fuera de su organización. | Must |
| CP-RNF-05-02 | Personal DPRIME sin autorización no accede | Evidencia de un cliente; personal DPRIME sin autorización registrada | 1) Personal DPRIME intenta acceder a la evidencia. | Acceso denegado. | 0% de accesos a evidencia de un cliente sin autorización registrada. | Must |
| CP-RNF-05-03 | Personal DPRIME con autorización registrada accede y queda registrado | Autorización expresa y registrada del cliente para un incidente | 1) Personal DPRIME accede a la evidencia autorizado. 2) Consultar el registro. | Acceso concedido y registrado con la autorización asociada. | Consistente con RF-32 / RF-47. | Must |

### RNF-06 — Consistencia del conteo de validaciones

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-06-01 | Contador visible = contador de facturación en todo momento | Cliente con validaciones registradas y en curso | 1) Consultar el contador visible al cliente y el usado para facturación en varios instantes, incluyendo durante el procesamiento de casos. | Ambos valores son idénticos en cada consulta. | Diferencia = 0 entre el contador visible al cliente y el usado para facturar, en cualquier instante de consulta. | Must |

### RNF-07 — Gobernanza / Segregación de funciones en umbrales

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-07-01 | Ningún umbral vigente con un único aprobador | Historial de cambios de umbral | 1) Auditar todos los cambios de umbral vigentes. 2) Verificar el número y el rol de los aprobadores de cada uno. | Todo cambio de umbral vigente tiene una segunda aprobación de un rol distinto al del solicitante. | 0% de cambios de umbral vigentes con un único aprobador registrado. | Must |

### RNF-08 — Usabilidad / Reducción de sesgo de anclamiento

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-08-01 | El puntaje se renderiza después de la evidencia y las alertas | Caso derivado; pantalla de revisión asistida | 1) Cargar la pantalla de revisión asistida múltiples veces (distintos casos). 2) Verificar el orden de renderizado en cada carga. | En cada carga, el componente de puntaje se renderiza después de la evidencia y las alertas. | El componente de puntaje debe renderizarse después de la evidencia y las alertas en el 100% de las cargas de la pantalla de revisión. | Should |

### RNF-09 — Cumplimiento normativo en eliminación de datos

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-09-01 | Toda solicitud de eliminación tiene resolución documentada | Conjunto de solicitudes de eliminación de un período | 1) Revisar todas las solicitudes de eliminación del período. 2) Verificar que cada una indique si el dato se eliminó o se conservó y por qué razón legal. | El 100% de las solicitudes tiene una resolución documentada (eliminado o conservado, con razón). | 100% de las solicitudes de eliminación con una resolución documentada (eliminado o conservado, con razón). | Must |

### RNF-10 — Portabilidad de integración (callback por cliente)

| ID | Objetivo | Precondiciones | Pasos | Resultado esperado | Criterio de aceptación | Prioridad |
|---|---|---|---|---|---|---|
| CP-RNF-10-01 | Configurar un canal de callback propio por cliente | Dos clientes distintos (A y B), ambos con casos en curso | 1) Configurar un callback para el cliente A hacia su sistema de origen. 2) Resolver un lote conocido de casos de A y de B. 3) Contar las llamadas recibidas en cada dirección. | El 100 % de los casos resueltos de A generan una llamada a la dirección configurada por A; ninguna llamada alcanza la dirección de otra organización. | 100 % de los casos resueltos de A con llamada a la dirección de A; 0 % de llamadas cruzadas hacia otra organización. | Could |

---

## 3. Matriz de cobertura

| Requisito | Casos de prueba | Requisito | Casos de prueba |
|---|---|---|---|
| RF-01 | CP-RF-01-01/02/03 | RF-29 | CP-RF-29-01 |
| RF-02 | CP-RF-02-01/02 | RF-30 | CP-RF-30-01 |
| RF-03 | CP-RF-03-01 | RF-31 | CP-RF-31-01 |
| RF-04 | CP-RF-04-01/02/03 | RF-32 | CP-RF-32-01/02 |
| RF-05 | CP-RF-05-01 | RF-33 | CP-RF-33-01 |
| RF-06 | CP-RF-06-01 | RF-34 | CP-RF-34-01 |
| RF-07 | CP-RF-07-01/02 | RF-35 | CP-RF-35-01 |
| RF-08 | CP-RF-08-01/02 | RF-36 | CP-RF-36-01 |
| RF-09 | CP-RF-09-01 | RF-37 | CP-RF-37-01 |
| RF-10 | CP-RF-10-01/02/03 | RF-38 | CP-RF-38-01/02 |
| RF-11 | CP-RF-11-01/02 | RF-39 | CP-RF-39-01 |
| RF-12 | CP-RF-12-01 | RF-40 | CP-RF-40-01 |
| RF-13 | CP-RF-13-01 | RF-41 | CP-RF-41-01/02 |
| RF-14 | CP-RF-14-01/02 | RF-42 | CP-RF-42-01/02 |
| RF-15 | CP-RF-15-01 | RF-43 | CP-RF-43-01 |
| RF-16 | CP-RF-16-01 | RF-44 | CP-RF-44-01/02 |
| RF-17 | CP-RF-17-01 | RF-45 | CP-RF-45-01 |
| RF-18 | CP-RF-18-01/02/03 | RF-46 | CP-RF-46-01/02 |
| RF-19 | CP-RF-19-01/02 | RF-47 | CP-RF-47-01 |
| RF-20 | CP-RF-20-01 | RF-48 | CP-RF-48-01/02/03 |
| RF-21 | CP-RF-21-01 | RF-49 | CP-RF-49-01 |
| RF-22 | CP-RF-22-01 | RF-50 | CP-RF-50-01/02 |
| RF-23 | CP-RF-23-01 | RF-51 | CP-RF-51-01 |
| RF-24 | CP-RF-24-01 | RF-52 | CP-RF-52-01/02 |
| RF-25 | CP-RF-25-01/02 | RF-53 | CP-RF-53-01 |
| RF-26 | CP-RF-26-01 | RF-54 | CP-RF-54-01 |
| RF-27 | CP-RF-27-01/02 | RF-55 | CP-RF-55-01 |
| RF-28 | CP-RF-28-01/02/03 | — | — |
| RNF-01 | CP-RNF-01-01/02/03 | RNF-06 | CP-RNF-06-01 |
| RNF-02 | CP-RNF-02-01 | RNF-07 | CP-RNF-07-01 |
| RNF-03 | CP-RNF-03-01 | RNF-08 | CP-RNF-08-01 |
| RNF-04 | CP-RNF-04-01 | RNF-09 | CP-RNF-09-01 |
| RNF-05 | CP-RNF-05-01/02/03 | RNF-10 | CP-RNF-10-01 |

**Totales:** 55 RF cubiertos por 79 casos de prueba funcionales; 10 RNF cubiertos por 15 casos de prueba extra-funcionales. **94 casos de prueba**, todos trazables a un requisito de `requisitosFuncionales_y_NoFuncionales.md`.

## 4. Notas de trazabilidad

- **Derivación estricta:** cada caso de prueba proviene del texto de un requisito RF/RNF y/o de su columna **Verificación**. No se usaron los casos de uso, el modelo de dominio ni las entrevistas como fuente directa (aunque el requisito los cite en su propia columna Fuente).
- **`Verificación = null`:** cuando el requisito no aporta un número, umbral o regla binaria, el criterio de aceptación del caso de prueba se limita a la comprobación cualitativa del texto y se marca explícitamente. No se inventaron umbrales (p. ej. la anticipación de la alerta de plazo en RF-20, el formato de exportación en RF-31, los mínimos por señal en RF-11).
- **Casos negativos / alternativos:** se incluyeron únicamente donde el requisito enuncia una prohibición o una métrica objetivo ("no debe", "nunca", "únicamente", "0%", "100%"): RF-03, RF-05, RF-07, RF-10, RF-12, RF-13, RF-14, RF-19, RF-23, RF-25, RF-27, RF-28, RF-32, RF-38, RF-41, RF-42, RF-46, RF-50, RF-55 y los RNF con criterio porcentual.
- **Requisitos con criterio parametrizable:** RF-11-02 depende de "mínimos por señal" que el requisito declara no definidos; el caso queda especificado pero no ejecutable con datos fijos hasta que esa configuración exista.
