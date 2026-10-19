# Actividad 1 grupal: Planeación de un sprint en el marco de trabajo Scrum

**Materia:** Metodología de Diseño y Gestión de Proyectos (MEXMIART08)
**Equipo:** Andrés Hernández Hernández · Néstor Alberto Malo Padilla · Bernabé Nava Maldonado · Fernando Mariano Vázquez Gutiérrez

> **Borrador v1 para revisión del equipo.** Las secciones 1 a 4 y 6 están redactadas. La sección 5 contiene solo guías: cada integrante escribe su propia reflexión. Al final hay notas de revisión que deben eliminarse antes de entregar.

---

## 1. Descripción del proyecto

FraudShield Analytics, empresa mexicana que ofrece soluciones de inteligencia artificial para prevenir el fraude financiero, desarrollará FraudShield Core v1.0 para la Red Interbancaria Latinoamericana (RILA). RILA es el cliente: agrupa a instituciones financieras de México, Colombia y Chile y convocó la licitación 2026-FSA-01. Hoy opera con un motor de reglas estáticas que no se adapta a la velocidad con la que el fraude migra a los canales digitales.

FraudShield Core v1.0 es un sistema de detección de fraude con tarjeta en tiempo real, basado en aprendizaje automático (XGBoost) y con explicaciones SHAP para auditoría. Su objetivo es detectar al menos el 90 % de las transacciones fraudulentas, mantener los falsos positivos por debajo del 2 % y responder en menos de 200 ms. El proyecto se ejecutará como un piloto de nueve meses en una institución participante. Este documento planea su primer sprint.

## 2. Asignación de roles Scrum

| Integrante | Rol (Guía Scrum 2020) | Responsabilidad principal en el sprint |
|---|---|---|
| Andrés Hernández Hernández | Product Owner | Maximizar el valor del producto: ordenar el *product backlog*, definir criterios de aceptación y representar a RILA |
| Néstor Alberto Malo Padilla | Scrum Master | Facilitar los eventos, eliminar impedimentos y cuidar que el equipo aplique Scrum |
| Bernabé Nava Maldonado | Developer | Construir el incremento: baseline de reglas y evaluación comparativa |
| Fernando Mariano Vázquez Gutiérrez | Developer | Construir el incremento: preparación de datos y modelo base |

Los cuatro integrantes forman un único Scrum Team. Los Developers se autogestionan y estiman el trabajo.

## 3. Product backlog

Estimación en *story points* (escala de Fibonacci), asignada por los Developers. Prioridad definida por el Product Owner según el valor para RILA. Los ítems aparecen en el orden en que se atenderían.

| ID | Historia de usuario | Criterio de aceptación principal | Prioridad | SP |
|---|---|---|---|---|
| HU-01 | Como **comité operativo del piloto**, quiero que los datos de evaluación estén separados cronológicamente de los de entrenamiento, para confiar en que las métricas reflejan el desempeño sobre fraudes futuros. | Partición 70/15/15 por fecha, sin traslape temporal y con tasa de fraude reportada por partición | Alta | 3 |
| HU-02 | Como **analista de prevención de fraude**, quiero medir el desempeño del sistema de reglas actual, para tener un punto de comparación objetivo. | Cinco reglas implementadas con supuestos documentados; recall, precisión, FPR, AUC-ROC y AUC-PR en el conjunto de prueba | Alta | 5 |
| HU-03 | Como **gerente de riesgos de RILA**, quiero un primer modelo de aprendizaje automático comparado contra las reglas, para saber desde la primera semana si el enfoque tiene potencial. | Regresión logística evaluada en el mismo conjunto de prueba que HU-02; tabla comparativa reproducible | Alta | 5 |
| HU-04 | Como **analista de prevención de fraude**, quiero que el modelo considere el comportamiento histórico de cada tarjeta (frecuencia, montos, dispositivo), para detectar patrones que una regla aislada no ve. | Al menos 40 variables derivadas documentadas, con cobertura ≥ 95 % después de imputar | Alta | 8 |
| HU-05 | Como **gerente de riesgos de RILA**, quiero un modelo que detecte al menos el 90 % de los fraudes con menos de 2 % de falsos positivos, para reducir pérdidas sin afectar a clientes legítimos. | XGBoost optimizado con recall ≥ 0,90 y FPR < 2 % en el conjunto de prueba | Alta | 8 |
| HU-06 | Como **institución participante**, quiero consultar el modelo como un servicio en tiempo real, para integrarlo al flujo de autorización de transacciones. | Latencia p99 < 200 ms bajo carga de prueba | Media | 13 |
| HU-07 | Como **auditor de cumplimiento**, quiero ver por qué se marcó una transacción como sospechosa, para resolver controversias de clientes. | Toda alerta muestra sus cinco variables SHAP principales y la versión del modelo | Media | 5 |
| HU-08 | Como **tarjetahabiente legítimo**, quiero que mi compra se procese aunque el modelo falle, para no sufrir rechazos por problemas técnicos. | Si el servicio no responde a tiempo, la transacción se evalúa con las reglas vigentes | Media | 5 |
| HU-09 | Como **analista de prevención de fraude**, quiero un tablero con alertas y métricas filtradas por mi perfil, para dar seguimiento sin ver información que no me corresponde. | Vistas diferenciadas por rol y métricas actualizadas por día | Baja | 8 |

## 4. Sprint backlog — Sprint 1 (una semana)

**Sprint Goal:** Demostrar, con un conjunto de prueba común y reproducible, si un primer modelo de aprendizaje automático supera al sistema de reglas de RILA, y así contar con la línea base que guiará los siguientes sprints.

**Historias seleccionadas:** HU-01, HU-02 y HU-03 (13 SP).

**Capacidad del equipo.** Supuesto: dos Developers de tiempo completo con 6 horas productivas al día.

| Concepto | Por Developer | Total |
|---|---|---|
| Capacidad bruta (5 días × 6 h) | 30 h | 60 h |
| Eventos Scrum (Planning 2 h, Dailies 1,25 h, Review 1 h, Retrospectiva 0,75 h) | −5 h | −10 h |
| **Capacidad neta** | **25 h** | **50 h** |
| Trabajo planeado | B: 21 h · F: 23 h | 44 h (holgura del 12 %) |

| ID | Tarea | Historia | Responsable | Horas |
|---|---|---|---|---|
| T1.1 | Descargar el dataset IEEE-CIS, unir las tablas de transacciones e identidad y optimizar tipos de datos | HU-01 | Fernando | 4 |
| T1.2 | Análisis exploratorio: balance de clases, valores faltantes y rango temporal | HU-01 | Fernando | 4 |
| T1.3 | Partición temporal 70/15/15 por `TransactionDT` | HU-01 | Fernando | 3 |
| T1.4 | Prueba automatizada de la partición (sin traslape, proporciones correctas) | HU-01 | Bernabé | 2 |
| T2.1 | Definir aproximaciones de las cinco reglas con las variables disponibles y documentarlas | HU-02 | Bernabé | 4 |
| T2.2 | Implementar el motor de reglas con puntaje ponderado | HU-02 | Bernabé | 5 |
| T2.3 | Calcular métricas del baseline en el conjunto de prueba | HU-02 | Bernabé | 3 |
| T2.4 | Revisión cruzada del código de reglas | HU-02 | Fernando | 2 |
| T2.5 | Documentar supuestos y limitaciones del baseline | HU-02 | Bernabé | 2 |
| T3.1 | Pipeline de preprocesamiento ajustado solo con datos de entrenamiento | HU-03 | Fernando | 5 |
| T3.2 | Entrenar regresión logística con balanceo de clases aplicado solo en entrenamiento | HU-03 | Fernando | 3 |
| T3.3 | Evaluar el modelo y construir la tabla comparativa contra el baseline | HU-03 | Bernabé | 3 |
| T3.4 | Revisión cruzada del pipeline para descartar fuga de información | HU-03 | Bernabé | 2 |
| T3.5 | Empaquetar notebook reproducible (semilla fija, dependencias, README) | HU-03 | Fernando | 2 |
| | **Total** | | | **44** |

**Definition of Done.** Código en el repositorio con revisión cruzada aprobada, resultados reproducibles con semilla fija, métricas calculadas sobre el conjunto de prueba común y supuestos documentados.

## 5. Reflexión individual por integrante

> **Guías, no texto.** Cada integrante redacta su párrafo (150–200 palabras) respondiendo: ¿qué tareas realizarías en tu rol?, ¿qué retos anticipas?, ¿cómo contribuirías al equipo? Escríbanlo en primera persona y ancládenlo en las tareas y eventos concretos de este sprint.

### 5.1 Andrés Hernández Hernández — Product Owner
- **Tareas:** ordenar el backlog y justificar por qué HU-01 a HU-03 van primero; aclarar los criterios de aceptación en la Planning; aceptar o rechazar el incremento en la Review.
- **Retos posibles:** representar a un cliente (RILA) que no está presente; resistir la presión de "ir directo a XGBoost" sin línea base; que HU-06 (13 SP) deba partirse antes de entrar a un sprint.
- **Contribución:** cómo traduce las metas de RILA (recall, FPR, latencia) en prioridades del backlog.

### 5.2 Néstor Alberto Malo Padilla — Scrum Master
- **Tareas:** facilitar los cuatro eventos con su timebox; detectar impedimentos (acceso al dataset, entorno de cómputo); cuidar que la Daily sea de coordinación y no de reporte de estado.
- **Retos posibles:** la dependencia entre T1.3 (partición) y las tareas de Bernabé; un equipo nuevo sin velocidad histórica; evitar actuar como jefe de proyecto.
- **Contribución:** servicio al equipo y al Product Owner, según la Guía 2020; qué haría si el Sprint Goal se pone en riesgo a mitad de semana.

### 5.3 Bernabé Nava Maldonado — Developer
- **Tareas:** T1.4, T2.1–T2.3, T2.5, T3.3 y T3.4 (21 h).
- **Retos posibles:** el dataset público no trae un identificador explícito de cliente ni el país de forma directa, así que las reglas del baseline requieren aproximaciones (`card1`–`card6`, `addr1`/`addr2`, `dist1`, `DeviceInfo`); depender de la partición de Fernando para empezar a medir; detectar fuga de información en la revisión cruzada.
- **Contribución:** cómo tu experiencia en la industria aporta a revisiones de código y pruebas; en qué difiere tomar tareas por autogestión de lo que viviste en proyectos reales.

### 5.4 Fernando Mariano Vázquez Gutiérrez — Developer
- **Tareas:** T1.1–T1.3, T2.4, T3.1, T3.2 y T3.5 (23 h).
- **Retos posibles:** volumen del dataset (590,540 transacciones × 433 variables) y uso de memoria; ser el primer eslabón de la cadena de dependencias; ajustar el preprocesamiento sin contaminar la prueba.
- **Contribución:** cómo garantiza la reproducibilidad para que todo el equipo trabaje sobre los mismos datos.

## 6. Conclusión grupal

Planear este sprint nos obligó a traducir una propuesta organizada en fases mensuales a un ciclo de una semana con un entregable verificable. Nuestra primera decisión fue mantener el Gantt de nueve meses como hoja de ruta ante RILA y ejecutar el trabajo en sprints. Este enfoque híbrido conserva la visibilidad que exige un cliente institucional sin renunciar a la inspección y adaptación de Scrum. La segunda decisión fue dedicar el Sprint 1 a una línea base medible: antes de optimizar cualquier modelo, el equipo necesita saber si el aprendizaje automático supera a las reglas en el mismo conjunto de prueba.

También aprendimos que estimar corresponde a quien construye. Los Developers asignaron los *story points*, y verificamos que las 44 horas planeadas cupieran en una capacidad real de 50 horas, ya descontados los eventos de Scrum. Al planear el baseline identificamos que el dataset público no permite reproducir todas las reglas de forma directa, un riesgo que la propuesta original no hacía visible. Finalmente, separar las responsabilidades del Product Owner, el Scrum Master y los Developers aclaró quién decide qué se construye, quién protege el proceso y quién define cómo hacerlo.

## Referencias

Hernández Hernández, A., Malo Padilla, N. A., Nava Maldonado, B., & Vázquez Gutiérrez, F. M. (2026). *Detección de fraude transaccional en tiempo real: Propuesta de solución de inteligencia artificial para la Red Interbancaria Latinoamericana* [Trabajo académico no publicado]. Universidad Internacional de La Rioja en México.

IEEE Computational Intelligence Society & Vesta Corporation. (2019). *IEEE-CIS fraud detection* [Conjunto de datos]. Kaggle. https://www.kaggle.com/c/ieee-fraud-detection

Schwaber, K., & Sutherland, J. (2020). *The Scrum Guide: The definitive guide to Scrum: The rules of the game*. Scrum.org. https://scrumguides.org/scrum-guide.html

---

## Notas para revisión (eliminar antes de entregar)

1. **Extensión.** Se respetan los límites por sección. Falta que el profesor aclare el "máximo 500 palabras".
2. **Reutilización de la propuesta.** Se cita como trabajo previo del equipo. Confirmen con el profesor que pueden reutilizarla y verifiquen el año y la materia de origen en la referencia.
3. **Velocidad.** Como es el primer sprint, no hay velocidad histórica. Los 13 SP se validaron solo contra la capacidad en horas.
4. **HU-06 (13 SP)** es demasiado grande para un sprint de una semana y deberá partirse en un refinamiento posterior. Dejarla así es intencional, porque muestra el criterio del PO, pero conviene mencionarlo en la reflexión de Andrés.
5. **Roles.** Andrés y Néstor no tienen tareas de desarrollo. Si el equipo decide que también actúen como Developers, hay que recalcular la capacidad.
6. **Formato final.** Pasar a Word con Calibri 12 e interlineado 1,5.
