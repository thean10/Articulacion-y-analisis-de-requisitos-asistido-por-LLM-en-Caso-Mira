# BACKLOG — MIRA (Historias de Usuario)

Derivado de `CasosDeUso_MIRA.md` y `requisitosFuncionales_y_NoFuncionales.md`, y verificado contra `CasosDePrueba_MIRA.md` y `ModeloDominio_MIRA.md`.

**Regla de derivación (misma disciplina que los artefactos previos):** cada historia se traza a un caso de uso (UC-XX), a uno o más requisitos (RF-XX / RNF-XX) y a los casos de prueba que la cubren (CP-…). Los criterios de aceptación se toman del texto del requisito y de su columna **Verificación**; cuando esa columna es `null`, el criterio se limita a la comprobación cualitativa del texto y se marca como **(criterio cualitativo — sin umbral en la fuente)**. No se inventan umbrales, formatos ni reglas que las fuentes no den.

- **Épicas:** 17, alineadas 1:1 con los 18 casos de uso (UC-01 y UC-13 comparten la épica de ciclo de vida del caso).
- **Historias:** 55 funcionales (una por RF) + 10 transversales no funcionales (una por RNF).
- **Estimación:** se deja pendiente de *planning poker* con el equipo. La columna **Prioridad** se hereda del requisito de origen y se expresa en **MoSCoW** (Must / Should / Could / Won't), con el criterio de asignación declarado en `requisitosFuncionales_y_NoFuncionales.md`.

---

## Definition of Ready (DoR) — cuándo una historia entra a un sprint

Una historia está lista para planificarse cuando:

1. Tiene narrativa (rol / objetivo / beneficio), criterios de aceptación y trazabilidad UC/RF/CP completos.
2. Sus dependencias con otras historias están identificadas (columna *Depende de*).
3. El equipo entiende el alcance y puede estimarla sin ambigüedad bloqueante; toda `null` de verificación tiene una decisión de producto asociada o está explícitamente aceptada como criterio cualitativo.
4. Los datos de prueba necesarios (evidencia de ejemplo, flujos, umbrales, cuentas por rol) están disponibles o su creación está incluida en la historia.
5. Cabe en un sprint; si no, se divide.

---

## Definition of Done (DoD) — global, aplica a todas las historias

Una historia está terminada cuando:

1. **Código:** revisado por par, fusionado a la rama principal, build verde, sin *warnings* nuevos.
2. **Criterios de aceptación:** todos verificados en ambiente de prueba por alguien distinto de quien desarrolló.
3. **Pruebas:** todos los casos de prueba trazados a la historia (CP-RF-… / CP-RNF-…) ejecutan y pasan; los de regla binaria o porcentaje (0 % / 100 %) están automatizados; sin regresiones en la suite existente.
4. **Auditoría:** cada evento relevante de la historia genera una entrada en el Rastro de Auditoría, inalterable, con actor, *timestamp*, estado antes/después y versiones de módulo/flujo cuando corresponda (RF-06).
5. **Aislamiento multicliente:** se verificó que la funcionalidad no expone ni permite acceder a datos de otra organización (RNF-05).
6. **Seguridad y datos personales:** evidencia y datos personales cifrados en tránsito y en reposo; accesos registrados (quién vio qué y cuándo); ningún dato personal o sensible en URL, registros ni parámetros.
7. **Reproducibilidad:** si la historia produce puntaje o decisión, quedan registradas las versiones de módulos y de flujo usadas, y el atributo `reproducible` refleja si el resultado es determinista (RNF-04).
8. **Trazabilidad y documentación:** matriz UC → Épica → HU → RF → CP actualizada; ayuda de usuario y/o contrato de la interfaz de programación al día; notas de versión del flujo o del módulo si aplica.
9. **RNF aplicables:** se cumplen los requisitos no funcionales que toquen la historia (rendimiento, control de acceso, consistencia, usabilidad/anti-sesgo, cumplimiento normativo).
10. **Aceptación:** demo al *Product Owner* y aceptación explícita.

---

## Épicas

| Épica | Nombre | UC | Historias | Prioridad global |
|---|---|---|---|---|
| EP-01 | Ingesta de evidencia y ciclo de vida del caso | UC-01, UC-13 | HU-01 … HU-06 | Must/Should |
| EP-02 | Procesamiento automático y motor de decisión | UC-02 | HU-07 … HU-14 | Must |
| EP-03 | Revisión asistida (bandeja del operador) | UC-03 | HU-15 … HU-20 | Must/Should |
| EP-04 | Diseño y publicación de flujos sin código | UC-04 | HU-21 … HU-26 | Must/Should |
| EP-05 | Gobierno de umbrales de decisión | UC-05 | HU-27 … HU-29 | Must |
| EP-06 | Auditoría, trazabilidad y reconstrucción de decisiones | UC-06 | HU-30 … HU-32 | Must |
| EP-07 | Paneles e indicadores de operación | UC-07 | HU-33 | Should |
| EP-08 | Corrección de datos leídos por la plataforma | UC-08 | HU-34 … HU-35 | Should |
| EP-09 | Versionado y comparación de módulos de análisis | UC-09 | HU-36 … HU-38 | Must/Should/Could |
| EP-10 | Medición de consumo y facturación | UC-10 | HU-39 … HU-41 | Must |
| EP-11 | Autenticación y control de acceso por rol | UC-11 | HU-42 … HU-43 | Must/Could |
| EP-12 | Datos personales y derecho de eliminación | UC-12 | HU-44 … HU-46 | Must |
| EP-13 | Acceso controlado de personal de DPRIME | UC-14 | HU-47 | Must |
| EP-14 | Notificaciones y alertas operativas | UC-15 | HU-48 … HU-50 | Should/Could |
| EP-15 | Incorporación de clientes con plantillas de industria | UC-16 | HU-51 … HU-52 | Should/Could |
| EP-16 | Resiliencia ante caída de servicios externos | UC-17 | HU-53 … HU-54 | Must/Should |
| EP-17 | Gestión del límite de llamadas (rate limit) de la API | UC-18 | HU-55 | Must |
| EP-NFR | Restricciones transversales no funcionales | — | HU-NFR-01 … HU-NFR-10 | Must/Should/Could |

---

## EP-01 — Ingesta de evidencia y ciclo de vida del caso

### HU-01 — Ingreso de evidencia por múltiples canales
**Prioridad:** Must · **Depende de:** — · **UC-01 (paso 1) · RF-01 · CP-RF-01-01/02/03**

> **Como** sistema del cliente (vía API), operador (carga manual) o proceso periódico (casilla/carpeta),
> **quiero** ingresar evidencia de un caso por cualquiera de los tres canales,
> **para** que toda la evidencia disponible sobre un hecho llegue a la plataforma sin importar su origen.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un canal API habilitado, cuando envío una pieza de evidencia legible, entonces se acepta y queda asociada a un caso (nuevo o existente).
- Dado un usuario autenticado con acceso a carga manual, cuando sube un archivo desde el navegador y confirma, entonces la evidencia se acepta y queda asociada a un caso.
- Dada una casilla/carpeta configurada como canal, cuando se deposita un archivo, entonces la plataforma lo recoge en su ciclo de revisión periódica y lo asocia a un caso.
- Los tres canales están disponibles simultáneamente para la organización.

### HU-02 — Verificación de legibilidad previa al análisis
**Prioridad:** Must · **Depende de:** HU-01 · **UC-01 (paso 2) · RF-02 · CP-RF-02-01/02**

> **Como** plataforma,
> **quiero** verificar que cada archivo de evidencia sea legible y utilizable antes de analizarlo,
> **para** no ejecutar módulos sobre material que no puede producir un resultado válido.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dada una evidencia legible, cuando ingresa, entonces se marca como verificada y solo entonces se envía a los módulos de análisis.
- Dada una evidencia no utilizable, cuando ingresa, entonces ningún módulo de análisis se ejecuta sobre ella.
- El rastro del caso refleja que la verificación de legibilidad precede a cualquier ejecución de módulo.

### HU-03 — Rechazo de evidencia ilegible con razón comprensible
**Prioridad:** Should · **Depende de:** HU-02 · **UC-01 (flujo alt.) · RF-03 · CP-RF-03-01**

> **Como** actor que ingresa evidencia,
> **quiero** que un archivo no legible sea rechazado con una explicación en lenguaje no técnico,
> **para** entender qué corregir en vez de enfrentar un fallo silencioso.

**Criterios de aceptación** *(criterio cualitativo — la fuente no define cómo medir "comprensible")*
- Dado un archivo corrupto o ilegible, cuando se ingresa, entonces el sistema lo rechaza explícitamente.
- El rechazo va acompañado de una razón legible para una persona no técnica.
- El caso/archivo no queda en procesamiento silencioso ni el sistema falla sin aviso.

### HU-04 — Identificador de caso único, permanente y estable
**Prioridad:** Must · **Depende de:** HU-01 · **UC-01 (pasos 3-4), UC-13 (pasos 1-3) · RF-04 · CP-RF-04-01/02/03**

> **Como** operador y como sistema del cliente,
> **quiero** que cada caso tenga un identificador único y permanente que no cambie al agregar evidencia o reevaluar,
> **para** poder referenciar y auditar el mismo caso a lo largo de todo su ciclo de vida.

**Criterios de aceptación**
- Dado un hecho sin caso previo, cuando ingresa la primera evidencia, entonces se crea un caso con identificador único y permanente.
- Dado un caso abierto, cuando ingresa evidencia adicional sobre el mismo hecho, entonces se asocia al caso existente y el identificador no cambia.
- Dada evidencia adicional que dispara reevaluación, cuando el caso se vuelve a evaluar con la información nueva sin perder lo ya concluido, entonces el identificador es idéntico antes y después.
- Un caso aprobado automáticamente no se vuelve a revisar por evidencia posterior (se aplica HU-05).

### HU-05 — Caso nuevo vinculado ante evidencia sobre un caso cerrado
**Prioridad:** Must · **Depende de:** HU-04 · **UC-01 (flujo alt.), UC-13 (flujo alt.) · RF-05 · CP-RF-05-01**

> **Como** plataforma,
> **quiero** abrir un caso nuevo vinculado al anterior cuando llega evidencia sobre un caso ya cerrado,
> **para** preservar la decisión ya tomada y su rastro sin reabrir un caso cerrado.

**Criterios de aceptación**
- Dado un caso en estado CERRADO, cuando ingresa evidencia sobre el mismo hecho, entonces el caso cerrado no cambia de estado.
- Se genera un caso nuevo con identificador distinto y una referencia explícita al caso original.

### HU-06 — Registro de eventos del caso en el rastro de auditoría
**Prioridad:** Must · **Depende de:** HU-01 · **UC-01 (paso 5), UC-02 (paso 6), UC-06 (pasos 2-3) · RF-06 · CP-RF-06-01**

> **Como** responsable de cumplimiento,
> **quiero** que cada evento relevante de un caso quede registrado en el rastro de auditoría,
> **para** poder reconstruir después todo lo que ocurrió sobre el caso.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un caso procesado de extremo a extremo, cuando consulto su rastro, entonces contiene, como mínimo, entradas para: ingreso de evidencia, ejecución de módulos y decisión.
- Cada entrada es inalterable y registra actor responsable, timestamp y estado antes/después.
- Ni ADMIN ni personal de DPRIME pueden modificar o eliminar una entrada del rastro.

---

## EP-02 — Procesamiento automático y motor de decisión

### HU-07 — Ejecución de los módulos activos del flujo vigente
**Prioridad:** Must · **Depende de:** HU-02 · **UC-02 (paso 1) · RF-07 · CP-RF-07-01/02**

> **Como** motor de análisis multimodal,
> **quiero** ejecutar exactamente los módulos de análisis activos del flujo vigente sobre la evidencia del caso,
> **para** que el análisis corresponda a la configuración que el cliente publicó.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un caso con evidencia legible y un flujo vigente con módulos activos, cuando se procesa, entonces se ejecutan exactamente esos módulos activos.
- Dado un flujo con módulos inactivos o deprecados, cuando se procesa un caso, entonces esos módulos no se ejecutan.

### HU-08 — Cálculo del puntaje de confianza con desglose
**Prioridad:** Must · **Depende de:** HU-07 · **UC-02 (pasos 2-3) · RF-08 · CP-RF-08-01/02**

> **Como** plataforma,
> **quiero** calcular para cada caso un puntaje de confianza entre 0 y 100 acompañado del desglose de las señales que lo componen,
> **para** que ninguna decisión se apoye en un número sin explicación.

**Criterios de aceptación**
- Dado un caso con evidencia legible y módulos activos, cuando se procesa, entonces el puntaje está dentro del rango [0,100] en el 100% de los casos con puntaje calculado.
- Todo caso con puntaje calculado presenta un desglose de componentes no vacío (qué señal empuja hacia arriba y hacia abajo).
- El puntaje registra las versiones de módulos usadas para su cálculo.

### HU-09 — Comparación del puntaje contra los umbrales configurados
**Prioridad:** Must · **Depende de:** HU-08, HU-27 · **UC-02 (paso 4) · RF-09 · CP-RF-09-01**

> **Como** plataforma,
> **quiero** comparar el puntaje del caso contra los umbrales configurados por el cliente (aprobación, derivación, escalamiento),
> **para** determinar la decisión que corresponde al caso.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dados casos con puntajes que caen en cada rango, cuando se procesan, entonces la decisión (aprobación / derivación / escalamiento) corresponde al rango de umbral en que cae el puntaje.
- La decisión registra qué configuración de umbral (`Umbral_id`) se aplicó.

### HU-10 — Aprobación automática solo con puntaje alto y sin fraude
**Prioridad:** Must · **Depende de:** HU-09 · **UC-02 (paso 5) · RF-10 · CP-RF-10-01/02/03**

> **Como** plataforma,
> **quiero** aprobar un caso automáticamente únicamente cuando el puntaje supera el umbral de aprobación y no hay ninguna señal de fraude activa,
> **para** automatizar solo los casos inequívocamente favorables.

**Criterios de aceptación**
- Dado un caso con puntaje ≥ umbral de aprobación y sin señal de fraude activa, cuando se procesa, entonces se aprueba automáticamente.
- 0% de casos con decisión de aprobación automática que tengan una señal de fraude activa asociada.
- Dado un caso con puntaje bajo el umbral de aprobación, cuando se procesa, entonces no se aprueba automáticamente.

### HU-11 — Derivación por rango intermedio o señal bajo su mínimo
**Prioridad:** Must · **Depende de:** HU-09 · **UC-02 (flujo alt.) · RF-11 · CP-RF-11-01/02**

> **Como** plataforma,
> **quiero** derivar a revisión asistida el caso cuyo puntaje cae en rango intermedio, o cuya señal individual está bajo su mínimo aunque el total lo permita,
> **para** que un humano resuelva los casos que la automatización no puede cerrar con seguridad.

**Criterios de aceptación** *(los mínimos por señal no están definidos en la fuente — parametrizable)*
- Dado un caso con puntaje en el rango de derivación, cuando se procesa, entonces se deriva a revisión asistida con `razonDerivacion` registrada.
- Dado un caso con puntaje total suficiente pero una señal individual bajo su mínimo (parámetro de configuración), cuando se procesa, entonces se deriva a revisión asistida pese al puntaje total.

### HU-12 — Derivar o escalar ante señal de fraude activa
**Prioridad:** Must · **Depende de:** HU-08 · **UC-02 (flujo alt.), Cap 3 · RF-12 · CP-RF-12-01**

> **Como** plataforma,
> **quiero** derivar o escalar todo caso con una señal de fraude activa, sin importar el puntaje total,
> **para** que las señales de fraude tengan precedencia sobre el puntaje.

**Criterios de aceptación**
- Dado un caso con señal de fraude activa y puntaje ≥ umbral de aprobación, cuando se procesa, entonces se deriva o escala; nunca se aprueba automáticamente.
- 0% de casos con señal de fraude activa resueltos con decisión de aprobación automática.

### HU-13 — Derivación ante módulo caído (sin aprobación parcial)
**Prioridad:** Must · **Depende de:** HU-07 · **UC-02 (flujo alt.) · RF-13 · CP-RF-13-01**

> **Como** plataforma,
> **quiero** derivar a revisión (nunca aprobar con información parcial) cuando un módulo falla o no está disponible, indicando qué faltó,
> **para** que la ausencia de una señal no se interprete como favorable.

**Criterios de aceptación**
- Dado un flujo con varios módulos activos y uno en error/indisponible, cuando se procesa un caso, entonces se deriva a revisión indicando el módulo o la información que faltó.
- 0% de casos con un módulo en estado de error resueltos con decisión de aprobación automática.

### HU-14 — Prohibición del rechazo automático
**Prioridad:** Must · **Depende de:** HU-09 · **UC-02 (flujo alt.), DEC-07 · RF-14 · CP-RF-14-01/02**

> **Como** plataforma,
> **quiero** que ningún caso se rechace de forma automática y que todo rechazo tenga un operador humano como autor,
> **para** cumplir la exigencia legal de que las decisiones adversas requieren intervención humana.

**Criterios de aceptación**
- Dado un caso con puntaje bajo el umbral inferior, cuando se procesa, entonces se escala o deriva a revisión humana; no se rechaza automáticamente.
- 0% de decisiones con resultado "rechazado" sin un autor humano (operador) registrado.

---

## EP-03 — Revisión asistida (bandeja del operador)

### HU-15 — Pantalla única de revisión asistida
**Prioridad:** Must · **Depende de:** HU-11 · **UC-03 (paso 2) · RF-15 · CP-RF-15-01**

> **Como** operador,
> **quiero** ver en una sola pantalla la evidencia, el desglose de señales y la razón de la derivación,
> **para** resolver el caso sin saltar entre vistas.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un caso derivado en mi bandeja, cuando lo abro, entonces la pantalla muestra simultáneamente evidencia, desglose de señales y razón de derivación, sin navegación adicional.

### HU-16 — Ver el hallazgo de la plataforma en cada pieza de evidencia
**Prioridad:** Should · **Depende de:** HU-15 · **UC-03 (paso 3) · RF-16 · CP-RF-16-01**

> **Como** operador,
> **quiero** abrir cada pieza de evidencia y ver qué encontró la plataforma y en qué parte del documento o imagen,
> **para** validar el hallazgo contra la evidencia real.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un caso derivado, cuando abro una pieza de evidencia, entonces se indica/resalta qué encontró el módulo y en qué parte del documento o imagen.

### HU-17 — Puntaje mostrado al final (reducción de sesgo de anclamiento)
**Prioridad:** Should · **Depende de:** HU-15 · **UC-03 (paso 4), DEC-03 · RF-17 · CP-RF-17-01**

> **Como** operador,
> **quiero** que el puntaje de confianza aparezca al final de la pantalla, después de la evidencia y las alertas,
> **para** formar mi criterio sin condicionarme por el número.

**Criterios de aceptación**
- Dada la pantalla de revisión asistida, cuando la abro, entonces el componente de puntaje no es el primer elemento visible: se renderiza después de la evidencia y las alertas.

### HU-18 — El operador aprueba, rechaza o solicita antecedentes
**Prioridad:** Must · **Depende de:** HU-15 · **UC-03 (pasos 5-6) · RF-18 · CP-RF-18-01/02/03**

> **Como** operador,
> **quiero** aprobar, rechazar o solicitar antecedentes adicionales sobre un caso derivado, registrando mi decisión y su motivo,
> **para** resolver el caso dejando constancia de por qué.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un caso en revisión, cuando selecciono "aprobar" / "rechazar" / "solicitar antecedentes" e ingreso el motivo y confirmo, entonces la decisión y su motivo quedan registrados.
- En un rechazo, el operador queda registrado como autor responsable (consistente con HU-14).

### HU-19 — Marcado de discrepancia operador vs. plataforma
**Prioridad:** Should · **Depende de:** HU-18 · **UC-03 (flujo alt.) · RF-19 · CP-RF-19-01/02**

> **Como** equipo de modelos,
> **quiero** que los casos donde la decisión del operador difiere de la sugerencia de la plataforma queden marcados, sin alterar la decisión tomada,
> **para** revisar después el comportamiento de los modelos.

**Criterios de aceptación**
- 100% de los casos con decisión del operador distinta de la sugerencia tienen el indicador de discrepancia registrado.
- Dado un operador que decide igual que la sugerencia, cuando confirma, entonces el caso no se marca como discrepancia.
- El marcado no cambia la decisión del operador.

### HU-20 — Alerta por plazo próximo a vencer
**Prioridad:** Should · **Depende de:** HU-15 · **UC-03 (flujo alt.) · RF-20 · CP-RF-20-01**

> **Como** operador,
> **quiero** recibir una alerta cuando un caso derivado se acerca al vencimiento de su plazo comprometido,
> **para** resolverlo dentro del plazo pactado con el asegurado.

**Criterios de aceptación** *(criterio cualitativo — la fuente no especifica la anticipación)*
- Dado un caso derivado con plazo comprometido, cuando el tiempo restante se acerca al vencimiento, entonces el operador recibe una alerta antes de que el plazo venza.

---

## EP-04 — Diseño y publicación de flujos sin código

### HU-21 — Creación de flujo conectando bloques sin código
**Prioridad:** Must · **Depende de:** — · **UC-04 (pasos 1-2) · RF-21 · CP-RF-21-01**

> **Como** diseñador de flujo,
> **quiero** armar el proceso arrastrando y conectando bloques (ingesta, análisis, cálculo de puntaje, reglas, decisión, integraciones) y configurarlos desde la interfaz,
> **para** adaptar el proceso a mi negocio sin escribir código.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un usuario con rol Diseñador de Flujo, cuando arrastra, conecta y configura bloques y guarda, entonces el flujo se crea y se guarda sin requerir escritura de código.

### HU-22 — Prueba del flujo con resultado paso a paso
**Prioridad:** Must · **Depende de:** HU-21 · **UC-04 (paso 3) · RF-22 · CP-RF-22-01**

> **Como** diseñador de flujo,
> **quiero** probar el flujo sobre casos de ejemplo antes de publicarlo y ver el resultado paso a paso,
> **para** verificar el comportamiento sin afectar producción.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un flujo en borrador y casos de ejemplo, cuando ejecuto la prueba, entonces el sistema muestra el resultado paso a paso por bloque y no publica el flujo.

### HU-23 — No modificar un flujo publicado en producción
**Prioridad:** Must · **Depende de:** HU-21 · **UC-04 (flujo alt.) · RF-23 · CP-RF-23-01**

> **Como** plataforma,
> **quiero** impedir la edición directa de un flujo publicado y forzar la creación de una versión nueva,
> **para** que producción nunca cambie sin pasar por prueba y publicación.

**Criterios de aceptación**
- Dado un flujo en estado PUBLICADO, cuando se intenta editarlo directamente, entonces el sistema rechaza la edición o fuerza la creación de una nueva versión en borrador.

### HU-24 — Casos en curso terminan con su versión inicial
**Prioridad:** Must · **Depende de:** HU-23 · **UC-04 (flujo alt.) · RF-24 · CP-RF-24-01**

> **Como** cliente,
> **quiero** que los casos ya en procesamiento terminen con la versión de flujo con la que empezaron aunque se publique una versión nueva,
> **para** que un caso no cambie de reglas a mitad de camino.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dados casos en estado PROCESANDO con la versión N, cuando se publica la versión N+1, entonces esos casos se completan con la versión N y los casos nuevos usan la N+1.

### HU-25 — No publicar un flujo sin módulo de análisis activo
**Prioridad:** Should · **Depende de:** HU-21 · **UC-04 (flujo alt.) · RF-25 · CP-RF-25-01/02**

> **Como** plataforma,
> **quiero** rechazar la publicación de un flujo que no tenga al menos un módulo de análisis activo,
> **para** que ningún flujo productivo pueda decidir sin analizar.

**Criterios de aceptación**
- Dado un flujo en borrador con 0 módulos de análisis activos, cuando se intenta publicar, entonces la publicación es rechazada.
- Dado un flujo con ≥ 1 módulo de análisis activo, cuando se publica, entonces la publicación se completa.

### HU-26 — Revertir un flujo a una versión anterior
**Prioridad:** Should · **Depende de:** HU-23 · **UC-04 (flujo alt.) · RF-26 · CP-RF-26-01**

> **Como** diseñador de flujo,
> **quiero** revertir un flujo a una versión anterior,
> **para** volver rápido a un comportamiento conocido sin deshacer las decisiones ya tomadas.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dada más de una versión del flujo, cuando selecciono una anterior y revierto, entonces esa versión queda vigente para casos nuevos.
- Revertir no altera las decisiones ya tomadas con la versión previamente vigente.

---

## EP-05 — Gobierno de umbrales de decisión

### HU-27 — Proponer un nuevo valor de umbral
**Prioridad:** Must · **Depende de:** — · **UC-05 (paso 1) · RF-27 · CP-RF-27-01/02**

> **Como** diseñador de flujo,
> **quiero** proponer un nuevo valor de umbral (aprobación, derivación, escalamiento) desde el diseñador de flujos,
> **para** ajustar la política de riesgo del cliente.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un usuario con rol Diseñador de Flujo, cuando propone un nuevo valor de umbral, entonces la propuesta queda registrada como pendiente de aprobación.
- Dado un usuario con otro rol, cuando intenta proponer un cambio de umbral, entonces la acción es rechazada.

### HU-28 — Segunda aprobación (Compliance) para dejar vigente el cambio
**Prioridad:** Must · **Depende de:** HU-27 · **UC-05 (paso 2), DEC-04 · RF-28 · CP-RF-28-01/02/03**

> **Como** responsable de Compliance,
> **quiero** que ningún cambio de umbral entre en vigencia sin una segunda aprobación de un rol distinto del solicitante,
> **para** garantizar segregación de funciones sobre la política de riesgo.

**Criterios de aceptación**
- 0% de cambios de umbral vigentes sin un segundo registro de aprobación distinto del solicitante.
- Dada una propuesta sin aprobación de Compliance, cuando se procesan casos, entonces sigue vigente el umbral anterior.
- Dada una propuesta aprobada por un usuario Compliance distinto del solicitante, cuando se procesan casos posteriores, entonces el nuevo umbral queda vigente.
- Dado el usuario que propuso el cambio, cuando intenta aprobar su propia propuesta, entonces la aprobación es rechazada.

### HU-29 — Registro auditable del cambio de umbral
**Prioridad:** Must · **Depende de:** HU-28 · **UC-05 (paso 3) · RF-29 · CP-RF-29-01**

> **Como** auditor,
> **quiero** que cada cambio de umbral quede registrado con su autor, fecha y valor anterior,
> **para** poder revisar la evolución de la política de riesgo.

**Criterios de aceptación**
- 100% de los registros de cambio de umbral contienen autor, fecha y valor anterior, todos no nulos.

---

## EP-06 — Auditoría, trazabilidad y reconstrucción de decisiones

### HU-30 — Reconstrucción de la decisión de un caso
**Prioridad:** Must · **Depende de:** HU-06 · **UC-06 (paso 2) · RF-30 · CP-RF-30-01**

> **Como** Compliance / Auditoría,
> **quiero** reconstruir la decisión de un caso tal como se tomó, con las versiones de módulos y de flujo vigentes en ese momento,
> **para** explicar ante un tercero cómo se decidió.

**Criterios de aceptación** *(criterio cuantitativo en HU-NFR-04)*
- Dado un caso decidido y posteriores actualizaciones de módulos y flujo, cuando reconstruyo la decisión, entonces la reconstrucción usa las versiones vigentes al momento de decidir, no las actuales.

### HU-31 — Exportación del rastro de auditoría completo
**Prioridad:** Must · **Depende de:** HU-30 · **UC-06 (pasos 3-4) · RF-31 · CP-RF-31-01**

> **Como** Compliance / Auditoría,
> **quiero** exportar el rastro de auditoría completo de un caso en un formato entregable a un tercero,
> **para** responder a un requerimiento regulatorio.

**Criterios de aceptación** *(la fuente no especifica el formato exacto)*
- Dado un caso con rastro registrado y un actor autorizado, cuando solicito la exportación, entonces el archivo contiene: evidencia, módulos ejecutados y su versión, reglas aplicadas, puntaje, decisión y quién intervino.
- El formato es entregable a un tercero.

### HU-32 — Restricción de acceso de DPRIME a evidencia y auditoría del cliente
**Prioridad:** Must · **Depende de:** HU-06 · **UC-06 (flujo alt.), UC-14 · RF-32 · CP-RF-32-01/02**

> **Como** cliente,
> **quiero** que el personal de DPRIME solo pueda acceder a la evidencia/auditoría de mis casos con autorización expresa y registrada, y solo para atender un incidente,
> **para** mantener el control sobre quién ve mis datos.

**Criterios de aceptación**
- 0% de accesos de personal DPRIME a evidencia de cliente sin un registro de autorización previo asociado.
- Dado personal DPRIME sin autorización, cuando intenta acceder, entonces el acceso es denegado y se registra como acceso denegado.
- Dada autorización expresa y registrada del cliente para un incidente, cuando personal DPRIME accede, entonces el acceso se concede y queda registrado (ver HU-47).

---

## EP-07 — Paneles e indicadores de operación

### HU-33 — Panel de indicadores de operación
**Prioridad:** Should · **Depende de:** HU-10, HU-11, HU-18 · **UC-07 (paso 2) · RF-33 · CP-RF-33-01**

> **Como** supervisor,
> **quiero** un panel con los indicadores clave de operación de mi organización,
> **para** identificar tendencias y anomalías sin ver casos individuales.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado que existen casos procesados en el período, cuando abro el panel, entonces muestra: volumen de casos por período y flujo, proporción resuelta automáticamente, proporción derivada, tiempo promedio de resolución, distribución de puntajes, tasa de detección de fraude y casos en que el operador contradijo a la plataforma.
- El panel no expone casos individuales (rol SUPERVISOR ve métricas y tendencias, no casos).

---

## EP-08 — Corrección de datos leídos por la plataforma

### HU-34 — El operador marca y corrige un dato mal leído
**Prioridad:** Should · **Depende de:** HU-15 · **UC-08 (pasos 1-2) · RF-34 · CP-RF-34-01**

> **Como** operador,
> **quiero** marcar como incorrecto un dato mal leído por un módulo (por ejemplo un monto o fecha) y corregirlo,
> **para** que el caso continúe con el valor correcto.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un caso en revisión con un dato mal leído, cuando lo marco como incorrecto, ingreso el valor correcto y confirmo, entonces la corrección se acepta y queda asociada al caso.

### HU-35 — La corrección no sobrescribe el valor original
**Prioridad:** Should · **Depende de:** HU-34 · **UC-08 (paso 3) · RF-35 · CP-RF-35-01**

> **Como** auditor,
> **quiero** que la corrección de un dato se registre junto al valor original sin sobrescribirlo,
> **para** que el rastro conserve lo que la plataforma leyó y lo que el humano corrigió.

**Criterios de aceptación**
- 0% de correcciones que eliminen o sobrescriban el valor original del registro.
- Dado un dato corregido, cuando consulto su registro, entonces el valor original permanece disponible junto al valor corregido.

---

## EP-09 — Versionado y comparación de módulos de análisis

### HU-36 — Comparar una versión nueva de módulo contra la vigente
**Prioridad:** Should · **Depende de:** — · **UC-09 (pasos 2-3) · RF-36 · CP-RF-36-01**

> **Como** equipo de ingeniería de modelos (DPRIME),
> **quiero** comparar el comportamiento de una versión nueva de un módulo contra la vigente sobre un conjunto de casos ya resueltos, antes de activarla,
> **para** conocer el impacto del cambio sin tocar producción.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dada una versión nueva de un módulo y un conjunto de casos ya resueltos, cuando ejecuto la comparación, entonces el sistema produce un reporte comparativo sin activar la versión nueva.

### HU-37 — Conteo de casos con decisión distinta entre versiones
**Prioridad:** Could · **Depende de:** HU-36 · **UC-09 (paso 3) · RF-37 · CP-RF-37-01**

> **Como** equipo de ingeniería de modelos,
> **quiero** ver en cuántos casos la decisión habría cambiado con la versión nueva,
> **para** cuantificar el riesgo del cambio antes de proponerlo al cliente.

**Criterios de aceptación**
- Dado el resultado de la comparación, cuando lo consulto, entonces incluye un conteo numérico de casos con decisión distinta entre ambas versiones.

### HU-38 — No alterar producción sin autorización explícita del cliente
**Prioridad:** Must · **Depende de:** HU-36 · **UC-09 (flujo alt.) · RF-38 · CP-RF-38-01/02**

> **Como** cliente,
> **quiero** que ninguna actualización de módulo altere el comportamiento de mi flujo en producción sin mi autorización explícita registrada,
> **para** mantener el control sobre qué versión decide mis casos.

**Criterios de aceptación**
- 0% de flujos en producción con cambio de versión de módulo sin un registro de autorización del cliente.
- Dada autorización explícita registrada del cliente, cuando se activa la versión nueva en producción, entonces se aplica y queda el registro de autorización asociado; los casos ya procesados con la versión anterior no se alteran.

---

## EP-10 — Medición de consumo y facturación

### HU-39 — Conteo de validaciones en tiempo real, consistente cliente/DPRIME
**Prioridad:** Must · **Depende de:** HU-10, HU-11, HU-18 · **UC-10 (paso 1), DEC-02 · RF-39 · CP-RF-39-01**

> **Como** cliente y como área comercial de DPRIME,
> **quiero** ver el conteo de validaciones en tiempo real y que ambos veamos exactamente el mismo número,
> **para** que la facturación no sea una caja negra.

**Criterios de aceptación**
- El número de validaciones mostrado al cliente y a DPRIME coincide (diferencia = 0) en cualquier momento de consulta.
- Una validación es un caso procesado exitosamente donde la plataforma entregó resultado con puntaje (automático, derivado o rechazo por humano).

### HU-40 — Detalle de consumo al cierre del período
**Prioridad:** Must · **Depende de:** HU-39 · **UC-10 (pasos 2-3) · RF-40 · CP-RF-40-01**

> **Como** área comercial de DPRIME,
> **quiero** que al cierre del período el sistema genere el detalle de consumo caso por caso con el criterio aplicado,
> **para** emitir la factura y poder revisarla antes.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un período con casos procesados, cuando ejecuto el cierre, entonces el sistema genera el detalle de consumo necesario para emitir la factura, con el criterio aplicado por caso.

### HU-41 — Exclusión de casos no cobrables
**Prioridad:** Must · **Depende de:** HU-39 · **UC-10 (flujo alt.), DEC-02 · RF-41 · CP-RF-41-01/02**

> **Como** cliente,
> **quiero** que no se me cobren los casos con evidencia ilegible ni los reintentos por error interno de la plataforma,
> **para** pagar solo por resultados entregados.

**Criterios de aceptación**
- 0% de casos no procesables (evidencia ilegible) incluidos en el conteo de validaciones facturables.
- 0% de reintentos por error interno de la plataforma incluidos en el conteo de validaciones facturables.

---

## EP-11 — Autenticación y control de acceso por rol

### HU-42 — Autenticación contra el repositorio propio y acceso por rol
**Prioridad:** Must · **Depende de:** — · **UC-11 (pasos 1-3) · RF-42 · CP-RF-42-01/02**

> **Como** usuario de una organización,
> **quiero** autenticarme con mis credenciales y recibir acceso según mi rol,
> **para** operar la plataforma con los permisos que me corresponden.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un usuario con cuenta activa propia, cuando ingresa credenciales válidas, entonces se le concede acceso con los permisos de su rol.
- Cuando ingresa credenciales inválidas, entonces el acceso es denegado.
- Un usuario con rol OPERADOR no puede modificar umbrales; un SUPERVISOR no ve casos individuales (restricciones de rol del modelo de dominio).

### HU-43 — Autenticación federada contra el directorio corporativo del cliente
**Prioridad:** Could · **Depende de:** HU-42 · **UC-11 (flujo alt.), DEC-08 · RF-43 · CP-RF-43-01**

> **Como** organización enterprise,
> **quiero** que mis usuarios se validen contra mi directorio corporativo cuando lo requiera,
> **para** no gestionar credenciales separadas en MIRA.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dada una organización configurada con autenticación federada, cuando un usuario suyo inicia sesión, entonces las credenciales se validan contra el directorio corporativo del cliente y no contra el repositorio propio.

---

## EP-12 — Datos personales y derecho de eliminación

### HU-44 — Ejercicio del derecho de eliminación con evaluación de obligación legal
**Prioridad:** Must · **Depende de:** — · **UC-12 (pasos 1-3) · RF-44 · CP-RF-44-01/02**

> **Como** titular del dato (gestionado por Compliance / Administrador de cuenta),
> **quiero** ejercer mi derecho a que se eliminen mis datos personales, evaluando primero si existe una obligación legal que prevalezca,
> **para** que se respete mi derecho salvo cuando la ley obligue a conservar.

**Criterios de aceptación** *(criterio cuantitativo en HU-NFR-09)*
- Dada una solicitud de eliminación sin obligación legal aplicable, cuando se procesa, entonces el sistema elimina los datos personales solicitados.
- Dada una solicitud con una obligación legal de conservación (por ejemplo, el período de retención de evidencia de siniestros), cuando se evalúa, entonces los datos se conservan y no se eliminan.

### HU-45 — Registro auditable de la resolución de la solicitud
**Prioridad:** Must · **Depende de:** HU-44 · **UC-12 (paso 4), DEC-01 · RF-45 · CP-RF-45-01**

> **Como** Compliance,
> **quiero** que cada solicitud de eliminación quede registrada con la razón de eliminación o de conservación,
> **para** demostrar el cumplimiento ante el regulador.

**Criterios de aceptación**
- 100% de las solicitudes de eliminación tienen un registro de razón (eliminado o conservado) no vacío.

### HU-46 — No eliminar evidencia con caso abierto dependiente
**Prioridad:** Must · **Depende de:** HU-44 · **UC-12 (flujo alt.) · RF-46 · CP-RF-46-01/02**

> **Como** plataforma,
> **quiero** no eliminar ninguna evidencia mientras exista un caso abierto que dependa de ella,
> **para** no dejar un caso en curso sin sustento.

**Criterios de aceptación**
- 0% de evidencias eliminadas mientras su caso asociado esté en un estado distinto de CERRADO.
- Dada evidencia asociada únicamente a casos CERRADOS y sin obligación legal de conservación, cuando se solicita la eliminación, entonces procede (sujeta a HU-44).

---

## EP-13 — Acceso controlado de personal de DPRIME

### HU-47 — Autorizar y registrar el acceso de DPRIME a evidencia de un cliente
**Prioridad:** Must · **Depende de:** HU-32 · **UC-14 (pasos 1-3) · RF-47 · CP-RF-47-01**

> **Como** administrador de cuenta del cliente,
> **quiero** que cada acceso de personal de DPRIME a la evidencia de un caso quede registrado con quién accedió, cuándo y con qué autorización,
> **para** tener trazabilidad completa de los accesos externos.

**Criterios de aceptación**
- 100% de los accesos de personal DPRIME quedan registrados con actor, fecha/hora y referencia a la autorización.
- El acceso solo se concede tras una autorización expresa y registrada del cliente, y únicamente para atender un incidente.

---

## EP-14 — Notificaciones y alertas operativas

### HU-48 — Detección de eventos y enrutamiento de notificaciones
**Prioridad:** Should · **Depende de:** — · **UC-15 (pasos 1-2) · RF-48 · CP-RF-48-01/02/03**

> **Como** operador / supervisor / equipo de DPRIME,
> **quiero** que el sistema detecte los eventos que requieren atención humana y notifique al destinatario correcto según el tipo de evento,
> **para** que cada quien reciba solo lo que le toca atender.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un evento "plazo por vencer", cuando se dispara, entonces la notificación llega al operador.
- Dado un evento "bandeja anómala", cuando se dispara, entonces la notificación llega al supervisor.
- Dado un evento "caída de servicio externo", cuando se dispara, entonces la notificación llega al equipo de DPRIME.

### HU-49 — Doble canal de notificación (correo e interfaz)
**Prioridad:** Should · **Depende de:** HU-48 · **UC-15 (paso 3) · RF-49 · CP-RF-49-01**

> **Como** destinatario de una alerta,
> **quiero** recibir la notificación por correo electrónico y verla también en la interfaz,
> **para** no depender de un solo canal.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un evento que requiere atención humana, cuando se dispara, entonces la notificación se envía por correo y también se muestra en la interfaz del destinatario.

### HU-50 — Callback al sistema de origen del caso
**Prioridad:** Could · **Depende de:** HU-48 · **UC-15 (flujo alt.) · RF-50 · CP-RF-50-01/02**

> **Como** integrador técnico del cliente,
> **quiero** que MIRA llame de vuelta a una dirección que yo configuro cuando ocurre un evento notificable de mis casos,
> **para** que mi sistema de origen reciba el aviso automáticamente.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dado un cliente con callback configurado, cuando ocurre un evento notificable de uno de sus casos, entonces el sistema realiza la llamada de vuelta a la dirección configurada.
- Dado un cliente sin callback configurado, cuando ocurre el mismo evento, entonces no se realiza ninguna llamada externa.

---

## EP-15 — Incorporación de clientes con plantillas de industria

### HU-51 — Plantillas preconfiguradas por caso de uso
**Prioridad:** Should · **Depende de:** HU-21 · **UC-16 (pasos 1-2) · RF-51 · CP-RF-51-01**

> **Como** diseñador de flujo de un cliente nuevo,
> **quiero** partir de una plantilla preconfigurada para mi industria (con módulos, reglas y umbrales sugeridos) y adaptar solo lo que difiere,
> **para** no construir el proceso desde cero.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dada una plantilla preconfigurada para el caso de uso del cliente, cuando creo el cliente nuevo a partir de ella, entonces se cargan los módulos, reglas y umbrales sugeridos de la plantilla y quedan editables.

### HU-52 — Tiempos de incorporación de un cliente nuevo
**Prioridad:** Could · **Depende de:** HU-51 · **UC-16 (pasos 3-4) · RF-52 · CP-RF-52-01/02**

> **Como** cliente nuevo,
> **quiero** procesar casos de prueba el mismo día de la firma y estar en producción dentro de dos semanas,
> **para** obtener valor de inmediato.

**Criterios de aceptación**
- Tiempo entre firma y primer caso de prueba ≤ 1 día.
- Tiempo entre firma y operación en producción ≤ 14 días.

---

## EP-16 — Resiliencia ante caída de servicios externos

### HU-53 — Reintento con criterio y cola sin descartar casos
**Prioridad:** Must · **Depende de:** HU-07 · **UC-17 (pasos 2-3), DEC-06 · RF-53 · CP-RF-53-01**

> **Como** plataforma,
> **quiero** reintentar con criterio la conexión a un servicio externo caído, seguir aceptando casos nuevos y mantenerlos en cola,
> **para** que ningún caso se pierda por una caída externa.

**Criterios de aceptación**
- 0% de casos descartados por caída de un servicio externo.
- Dado un servicio externo caído, cuando llegan casos nuevos, entonces se aceptan y quedan en cola; cuando el servicio vuelve, entonces los casos en cola se procesan.

### HU-54 — Aviso de procesamiento demorado
**Prioridad:** Should · **Depende de:** HU-53 · **UC-17 (paso 4) · RF-54 · CP-RF-54-01**

> **Como** cliente afectado y como DPRIME,
> **quiero** recibir aviso cuando haya procesamiento demorado por la caída de un servicio externo,
> **para** gestionar la expectativa con los asegurados.

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- Dada una demora por caída de un servicio externo, cuando se detecta, entonces tanto DPRIME como el cliente afectado reciben aviso del procesamiento demorado.

---

## EP-17 — Gestión del límite de llamadas (rate limit) de la API

### HU-55 — Encolamiento de llamadas sobre el límite
**Prioridad:** Must · **Depende de:** — · **UC-18 (flujo alt.), DEC-06 · RF-55 · CP-RF-55-01**

> **Como** integrador técnico del cliente,
> **quiero** que las llamadas que superen el límite por minuto se encolen (no se rechacen) y reciban de inmediato su posición en la cola,
> **para** no perder solicitudes en picos de carga.

**Criterios de aceptación**
- 0% de llamadas rechazadas por exceso de límite.
- 100% de las llamadas excedentes reciben de inmediato una respuesta de encolamiento con su posición en la cola (FIFO).

---

## EP-NFR — Restricciones transversales no funcionales

> Estas historias no agregan pantallas nuevas: fijan condiciones de calidad que **toda** entrega debe cumplir. Se planifican como historias técnicas cuando requieren trabajo dedicado (pruebas de carga, mecanismos de aislamiento, snapshots de configuración) y, además, sus criterios se incorporan al DoD de las historias funcionales que citan el mismo UC.

### HU-NFR-01 — Escalabilidad de la API sin rechazar el excedente
**Prioridad:** Must · **UC-18 · RNF-01 · CP-RNF-01-01/02/03**

**Criterios de aceptación**
- Prueba de carga sostenida de 100 llamadas/min en integración: 0 llamadas rechazadas.
- Prueba de carga sostenida de 1000 llamadas/min en producción: 0 llamadas rechazadas.
- El excedente por sobre el límite se encola (consistente con HU-55).

### HU-NFR-02 — Confiabilidad: cero casos perdidos ante caída de servicio externo
**Prioridad:** Must · **UC-17 · RNF-02 · CP-RNF-02-01**

**Criterios de aceptación**
- En una prueba de caída simulada de un servicio externo: 0 casos perdidos; se sigue aceptando casos nuevos durante la caída; al restablecerse, se procesan todos.

### HU-NFR-03 — Eficiencia operativa de incorporación ≤ 14 días
**Prioridad:** Could · **UC-16 · RNF-03 · CP-RNF-03-01**

**Criterios de aceptación**
- Operación de prueba disponible el día de la firma.
- Tiempo de incorporación ≤ 14 días corridos desde la firma hasta producción.

### HU-NFR-04 — Auditabilidad: reconstrucción exacta de decisiones
**Prioridad:** Must · **UC-06 · RNF-04 · CP-RNF-04-01**

**Criterios de aceptación**
- 0 discrepancias entre la configuración (versiones de módulo y flujo) registrada al momento de decidir y la mostrada al reconsultar el caso, aun después de cambiar versiones.

### HU-NFR-05 — Confidencialidad y control de acceso a la evidencia
**Prioridad:** Must · **UC-06, UC-14 · RNF-05 · CP-RNF-05-01/02/03**

**Criterios de aceptación**
- 0% de accesos a evidencia de un cliente realizados fuera de su organización.
- 0% de accesos de personal DPRIME a evidencia de un cliente sin autorización registrada.
- Todo acceso autorizado de DPRIME queda registrado con la autorización asociada.

### HU-NFR-06 — Consistencia del conteo de validaciones
**Prioridad:** Must · **UC-10 · RNF-06 · CP-RNF-06-01**

**Criterios de aceptación**
- Diferencia = 0 entre el contador de validaciones visible al cliente y el usado para facturar, en cualquier instante de consulta, incluso durante el procesamiento de casos.

### HU-NFR-07 — Segregación de funciones en cambios de umbral
**Prioridad:** Must · **UC-05 · RNF-07 · CP-RNF-07-01**

**Criterios de aceptación**
- 0% de cambios de umbral vigentes con un único aprobador registrado; toda vigencia tiene una segunda aprobación de un rol distinto al del solicitante.

### HU-NFR-08 — Usabilidad: el puntaje no se muestra antes que la evidencia
**Prioridad:** Should · **UC-03 · RNF-08 · CP-RNF-08-01**

**Criterios de aceptación**
- En el 100% de las cargas de la pantalla de revisión asistida, el componente de puntaje se renderiza después de la evidencia y las alertas.

### HU-NFR-09 — Cumplimiento normativo en eliminación de datos personales
**Prioridad:** Must · **UC-12 · RNF-09 · CP-RNF-09-01**

**Criterios de aceptación**
- 100% de las solicitudes de eliminación con una resolución documentada (eliminado o conservado, con razón legal).

### HU-NFR-10 — Portabilidad de integración: callback por cliente
**Prioridad:** Could · **UC-15 · RNF-10 · CP-RNF-10-01**

**Criterios de aceptación** *(criterio cualitativo — sin umbral en la fuente)*
- La configuración de callback es por cliente: configurar el callback del cliente A no afecta al cliente B.

---

## Matriz de trazabilidad — UC → Épica → Historias → RF/RNF

| UC | Épica | Historias | RF / RNF | Casos de prueba |
|---|---|---|---|---|
| UC-01 | EP-01 | HU-01, HU-02, HU-03, HU-04, HU-05, HU-06 | RF-01..06 | CP-RF-01..06 |
| UC-02 | EP-02 | HU-07 … HU-14 | RF-07..14 | CP-RF-07..14 |
| UC-03 | EP-03 | HU-15 … HU-20, HU-NFR-08 | RF-15..20, RNF-08 | CP-RF-15..20, CP-RNF-08-01 |
| UC-04 | EP-04 | HU-21 … HU-26 | RF-21..26 | CP-RF-21..26 |
| UC-05 | EP-05 | HU-27, HU-28, HU-29, HU-NFR-07 | RF-27..29, RNF-07 | CP-RF-27..29, CP-RNF-07-01 |
| UC-06 | EP-06 | HU-30, HU-31, HU-32, HU-NFR-04, HU-NFR-05 | RF-30..32, RNF-04, RNF-05 | CP-RF-30..32, CP-RNF-04/05 |
| UC-07 | EP-07 | HU-33 | RF-33 | CP-RF-33-01 |
| UC-08 | EP-08 | HU-34, HU-35 | RF-34, RF-35 | CP-RF-34-01, CP-RF-35-01 |
| UC-09 | EP-09 | HU-36, HU-37, HU-38 | RF-36..38 | CP-RF-36..38 |
| UC-10 | EP-10 | HU-39, HU-40, HU-41, HU-NFR-06 | RF-39..41, RNF-06 | CP-RF-39..41, CP-RNF-06-01 |
| UC-11 | EP-11 | HU-42, HU-43 | RF-42, RF-43 | CP-RF-42-01/02, CP-RF-43-01 |
| UC-12 | EP-12 | HU-44, HU-45, HU-46, HU-NFR-09 | RF-44..46, RNF-09 | CP-RF-44..46, CP-RNF-09-01 |
| UC-13 | EP-01 | HU-04, HU-05 | RF-04, RF-05 | CP-RF-04-03, CP-RF-05-01 |
| UC-14 | EP-13 | HU-47, HU-32, HU-NFR-05 | RF-47, RF-32, RNF-05 | CP-RF-47-01, CP-RF-32-01/02 |
| UC-15 | EP-14 | HU-48, HU-49, HU-50, HU-NFR-10 | RF-48..50, RNF-10 | CP-RF-48..50, CP-RNF-10-01 |
| UC-16 | EP-15 | HU-51, HU-52, HU-NFR-03 | RF-51, RF-52, RNF-03 | CP-RF-51-01, CP-RF-52-01/02, CP-RNF-03-01 |
| UC-17 | EP-16 | HU-53, HU-54, HU-NFR-02 | RF-53, RF-54, RNF-02 | CP-RF-53-01, CP-RF-54-01, CP-RNF-02-01 |
| UC-18 | EP-17 | HU-55, HU-NFR-01 | RF-55, RNF-01 | CP-RF-55-01, CP-RNF-01-01/02/03 |

**Cobertura:** 18 UC → 17 épicas → 65 historias (55 funcionales + 10 no funcionales) → 55 RF + 10 RNF → 94 casos de prueba. Toda historia tiene al menos un caso de prueba trazado.

---

## Notas metodológicas

- **Una historia por RF.** Se prefirió el mapeo 1:1 RF→HU para que el backlog sea directamente trazable a los artefactos previos y a la matriz de cobertura de `CasosDePrueba_MIRA.md`. Historias muy relacionadas (por ejemplo HU-04/HU-05, HU-32/HU-47) comparten dependencia y suelen planificarse juntas.
- **Criterios de aceptación = texto del requisito + columna Verificación + casos de prueba.** Donde la fuente da un umbral (0%, 100%, ≤ 1 día, ≤ 14 días, rango [0,100]) se usó literal. Donde la fuente dice `null` (mensajes "comprensibles", anticipación de alertas, formato de exportación, mínimos por señal) el criterio quedó cualitativo y marcado como tal; esas historias necesitan una **decisión de producto** antes de pasar a *Ready*.
- **RNF como historias transversales.** Cada RNF se expresó como historia propia (HU-NFR-XX) además de incorporarse al DoD global, porque varios exigen trabajo dedicado (pruebas de carga, aislamiento multi-tenant, snapshots de configuración para reconstrucción).
- **Prioridad heredada, no inventada.** La columna Prioridad viene del RF/RNF de origen. La estimación en puntos se deja para *planning poker*: las fuentes no dan tamaño y no se infirió ninguno.
- **Fuera de alcance por diseño (heredado de la nota de `requisitosFuncionales_y_NoFuncionales.md`):** disponibilidad 99,9%, tiempo de carga de pantalla < 2 s, residencia de datos en territorio nacional, ambientes separados, restauración de respaldos e idioma de interfaz no aparecen como historias porque no fueron formulados como paso o precondición de ningún caso de uso. Si el equipo los quiere en el backlog, deben entrar como historias nuevas con su propia fuente, no derivadas de este árbol.
