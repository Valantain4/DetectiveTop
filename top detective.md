---
equipo: Equipo 4
integrantes: Juan Andres Ramirez Castañeda - Daniela Cardona Madrid.
nivel: pregrado
caso: proyecto propio
nombre_del_sistema: Detective Top
formato_de_entrega: HTML
enlace_o_archivo_de_la_entrega: DetectiveTop_Bloque2.html
---

# Entrega Bloque 2: escenarios de calidad y ADR (texto plano)

## 1. Caso y ficha de contexto

**Cliente, en su voz:**
Detective Top es una aplicación web para investigar casos ficticios de misterio. Cada persona crea un perfil con contraseña. El jugador explora escenas, visita lugares, interroga personajes con preguntas predefinidas y, al final, acusa a un sospechoso. El sistema decide si la acusación es correcta y guarda el progreso y el resultado. La clasificación es una sola, global. Los tres primeros puestos, con la regla de desempate de esta ficha, reciben pistas para casos posteriores. La primera versión tiene como máximo 5 casos completos. Solo funciona con conexión. No hay aplicación nativa, chat, mensajería, pagos, compras ni publicidad. Perfiles, casos, decisiones, puntuaciones y progreso viven en una base de datos central.

**Anclas con cifras (cinco):**
1. Usuarios: 40 perfiles registrados.
2. Pico: 25 jugadores a la vez, durante una sesión de 90 minutos.
3. Disponibilidad: en esa sesión, las decisiones ya confirmadas se recuperan al recargar. Una escritura no confirmada no cuenta como decisión. Cero uso sin conexión.
4. Presupuesto: 0 servicios externos de pago. Una sola base central. Sin mensajería ni publicidad.
5. Regulación / restricción ancla: no hay norma sectorial ni pagos. El dato del perfil es el acceso con contraseña. La restricción que ancla la seguridad es la pista del top 3, no una ley externa.

**Tensión dominante, en una línea:**
En este sistema, Adecuación funcional (4) choca con Seguridad (4) porque explicar al jugador por qué su acusación fue rechazada mejora la función y la experiencia, pero revela parte de la estructura del caso y de la lógica de puntuación.

**Dónde está esto en la entrega:**
Sección 1 (Ficha de contexto) del HTML `DetectiveTop_Bloque2_Mejorado.html`.

---

## 2. Supuestos declarados (solo posgrado: mínimo cinco)

[No aplica: nivel pregrado. Se deja la sección vacía por estructura de la plantilla.]

---

## 3. Priorización con presupuesto de 18 puntos

- Adecuación funcional: 4 | Justificación: Prioridad de la etapa: el juego debe hacer lo que promete (explorar, interrogar, acusar, puntuar, clasificar, entregar pistas). Si una función principal falla, no hay producto que evaluar. No está en 5 porque las funciones están acotadas a 5 casos y a un flujo cerrado.
- Eficiencia de desempeño: 2 | Justificación: Con 25 concurrentes y 90 min, el sistema debe responder en tiempos razonables, pero no es la tensión de diseño de esta etapa. Se mide como restricción operativa (p95 < 2 s), no se optimiza.
- Compatibilidad: 1 | Justificación: Un cliente web y una base central. Sin chat, pagos ni sistemas externos.
- Usabilidad: 4 | Justificación: El jugador debe entender qué hacer sin fricción: elegir pregunta, interpretar respuesta, saber si la acusación fue aceptada, ver su puesto y qué le falta. En etapa temprana la experiencia del jugador es el criterio de adopción. No está en 5 porque el juego es de nicho y tolera cierta curva.
- Fiabilidad: 1 | Justificación: En etapa temprana se tolera reintentar y ver mensajes de error. La persistencia y la recuperación al recargar se validan como restricción operativa, no como eje. Baja a 1 para liberar el punto que sube Seguridad a 4.
- Seguridad: 4 | Justificación: La pista del top 3 y la integridad de la puntuación son la única fuente de riesgo real del producto. Se sube a 4 para cumplir la regla de tres atributos en 4 o más y porque las tensiones con AF y Usabilidad son medibles. No está en 5 porque el volumen es bajo (40 perfiles, casos ficticios, sin pagos) y no debe desplazar la experiencia del jugador como eje.
- Mantenibilidad: 1 | Justificación: La primera versión trae 5 casos ya cerrados. Agregar casos no es la tensión de esta entrega.
- Portabilidad: 1 | Justificación: Restricción del proyecto: solo web, sin aplicación nativa.

**Total:** 18

**Qué bajamos para poder subir el atributo más alto, y qué riesgo aceptamos:**
Para cumplir la regla de tres atributos en 4 o más con suma 18, la única distribución entera posible es 4,4,4,2,1,1,1,1. Subimos Seguridad de 3 a 4 (protege la pista del top 3 y la integridad de la puntuación) y bajamos Fiabilidad de 2 a 1 (en etapa temprana se tolera reintentar). Riesgo aceptado: la recuperación del progreso al recargar queda como restricción operativa, no como eje de diseño; si el cliente exige persistencia estricta, habría que renegociar la priorización.

**Dónde está esto en la entrega:**
Sección 2 (Priorización) del HTML.

---

## 4. Escenarios de calidad

### E1
- Atributo dominante: Adecuación funcional (4)
- Otro atributo que toca (si lo hay): Seguridad (4)
- Fuente del estímulo: Un jugador autenticado que envía una acusación al final de un caso
- Estímulo: Llega una acusación que cumple las condiciones del caso, o que no las cumple (faltan pistas obligatorias, sospechoso incorrecto, acciones incompletas)
- Artefacto: El servicio de acusación y puntuación
- Entorno: Sesión de 90 minutos, 25 jugadores a la vez, una base central, juego solo con conexión
- Respuesta: Distingue tres casos: (a) acusación válida → confirma el cierre, calcula la puntuación en el servidor y muestra el resultado con el desglose (aciertos, pistas usadas, tiempo); (b) acusación inválida por condición faltante → rechaza y explica al jugador qué condición falta, sin revelar la solución; (c) acusación inválida por sesión o por cierre duplicado → rechaza sin revelar reglas y ofrece reintentar. En (a) y (b) el mensaje es comprensible para el jugador sin conocer las reglas internas.
- Medida de la respuesta: En 30 acusaciones de prueba (10 válidas, 10 con condición faltante, 10 inválidas por sesión/duplicado): (1) 100 % de las válidas se confirman con puntuación calculada en servidor y desglose visible; (2) 100 % de las inválidas por condición faltante muestran un mensaje que nombra la condición faltante sin revelar la solución (verificado por revisión de 3 evaluadores independientes); (3) 0 de las inválidas por sesión/duplicado revelan reglas del caso; (4) en 100 acusaciones con 25 concurrentes, p95 < 2 s extremo a extremo.
- Supuestos de los que depende (S1, S2…) (solo posgrado): [No aplica: nivel pregrado]
- ¿Es crítico? (sí | no) y por qué: sí, porque su atributo dominante (Adecuación funcional, 4) está entre los más altos y cumplirlo cuesta Seguridad (4).
- Dónde está en la entrega: Sección 4 (Escenario 1) del HTML.

### E2
- Atributo dominante: Usabilidad (4)
- Otro atributo que toca (si lo hay): Adecuación funcional (4) y Seguridad (4)
- Fuente del estímulo: Un jugador nuevo, sin conocimiento previo del caso
- Estímulo: El jugador debe explorar una escena, interrogar a un personaje con preguntas predefinidas, visitar un lugar y llegar a la acusación, sin ayuda externa
- Artefacto: La interfaz web del caso y el servicio de progreso
- Entorno: Sesión de 90 minutos, 25 jugadores a la vez, una base central, juego solo con conexión
- Respuesta: La interfaz muestra en todo momento: (a) qué acciones están disponibles; (b) qué se completó y qué falta para poder acusar; (c) el estado de la sesión (conectado, guardando, error). Los mensajes de estado y error se filtran por perfil y caso: nunca muestran información de otros perfiles ni reglas internas que no correspondan al caso abierto. Distingue tres casos: (a) acción exitosa → avanza el estado visible; (b) acción inválida → mensaje claro y el jugador puede corregir sin perder progreso; (c) pérdida de conexión → mensaje explícito y opción de reintentar sin duplicar. El jugador puede completar el caso sin consultar documentación externa.
- Medida de la respuesta: En una prueba con 10 jugadores nuevos (no del equipo), cada uno completa un caso de principio a fin sin ayuda externa: (1) al menos 8 de 10 completan el caso sin pedir ayuda; (2) al menos 8 de 10 identifican correctamente qué les falta antes de acusar; (3) 0 jugadores pierden progreso confirmado por un error de la interfaz; (4) 0 mensajes de estado o error muestran información de otros perfiles o reglas de casos no abiertos (verificado por revisión de 3 evaluadores); (5) el tiempo medio para completar el caso no supera en más del 50 % el tiempo de referencia del equipo.
- Supuestos de los que depende (S1, S2…) (solo posgrado): [No aplica: nivel pregrado]
- ¿Es crítico? (sí | no) y por qué: sí, porque su atributo dominante (Usabilidad, 4) está entre los más altos y cumplirlo cuesta Adecuación funcional (4).
- Dónde está en la entrega: Sección 4 (Escenario 2) del HTML.

### E3
- Atributo dominante: Adecuación funcional (4)
- Otro atributo que toca (si lo hay): Seguridad (4)
- Fuente del estímulo: Un jugador autenticado que consulta las reglas y el estado de su caso
- Estímulo: El jugador pide ver qué condiciones debe cumplir para acusar y qué lleva completado; también intenta consultar las reglas de un caso que no tiene abierto
- Artefacto: El servicio de reglas del caso y el servicio de progreso
- Entorno: Sesión de 90 minutos, 25 jugadores a la vez, una base central, juego solo con conexión
- Respuesta: Devuelve al jugador las condiciones del caso en lenguaje comprensible (pistas obligatorias, acciones mínimas, sospechosos disponibles) y su estado de cumplimiento. Distingue tres casos: (a) consulta de su propio caso → muestra condiciones y estado; (b) consulta de un caso no abierto → rechaza sin revelar reglas; (c) consulta sin sesión → rechaza sin revelar reglas. Nunca devuelve el culpable ni la combinación exacta de respuestas correctas.
- Medida de la respuesta: En 30 consultas de prueba: (1) 100 % de las consultas al propio caso devuelven condiciones y estado completos (verificado contra la definición del caso); (2) 0 consultas a casos no abiertos o sin sesión devuelven reglas; (3) en 10 revisiones del contenido devuelto, 0 respuestas contienen el culpable o la combinación exacta de respuestas correctas (verificado por 3 evaluadores independientes); (4) en 100 consultas con 25 concurrentes, p95 < 2 s.
- Supuestos de los que depende (S1, S2…) (solo posgrado): [No aplica: nivel pregrado]
- ¿Es crítico? (sí | no) y por qué: sí, porque su atributo dominante (Adecuación funcional, 4) está entre los más altos y cumplirlo cuesta Seguridad (4).
- Dónde está en la entrega: Sección 4 (Escenario 3) del HTML.

---

## 5. Selección de escenarios críticos

**Escenarios críticos elegidos:** E1, E2 y E3.

**Por qué esos y no los demás (una línea):**
Los tres tienen atributo dominante en 4 y atributo pagado en 4 (AF vs Seguridad, Usabilidad vs AF, AF vs Seguridad); los pares con Desempeño (2) y Fiabilidad (1) se validan como restricciones operativas y no generan escenario crítico porque el atributo pagado no es alto.

---

## 6. ADR (uno por cada escenario crítico)

### ADR-1
- Título: Rechazar la acusación explicando la condición faltante
- Estado: Propuesto
- Escenario que lo origina: E1
- Context: igual al escenario indicado (sí; se copia íntegro)

**Opción A:**
- Qué haría el sistema: Rechazar la acusación con un mensaje genérico ("no se puede acusar todavía") sin decir qué falta.
- Gana: Seguridad: no revela ninguna regla del caso.
- Paga: Adecuación funcional: el jugador no sabe qué corregir; la función de acusar queda incompleta desde la experiencia.
- Atributo que sacrifica: Adecuación funcional (4).
- ¿Cumple la medida de la respuesta? (sí | no | depende) y por qué: no, porque la medida exige que el 100 % de las inválidas por condición faltante nombren la condición.
- Qué mediríamos para verificarla (solo posgrado): [No aplica: nivel pregrado]
- Decisión de producto o de política que encierra (a quién favorece o qué traslada a otro; o "ninguna"): Favorece la opacidad del sistema frente al jugador; traslada al jugador la carga de adivinar qué falta.

**Opción B:**
- Qué haría el sistema: Rechazar explicando la condición faltante en lenguaje del jugador (p. ej. "te falta interrogar al testigo"), calculada en el servidor a partir de las reglas del caso y el progreso confirmado; nunca revela el culpable ni la combinación de respuestas correctas.
- Gana: Adecuación funcional: la función de acusar guía al jugador y cierra el ciclo; Usabilidad: el jugador sabe qué hacer.
- Paga: Seguridad: se revela parte de la estructura del caso (qué condiciones existen), no la solución.
- Atributo que sacrifica: Seguridad (4).
- ¿Cumple la medida de la respuesta? (sí | no | depende) y por qué: depende; el diseño cumple la protección, pero falta la revisión de 3 evaluadores para confirmar que ningún mensaje revela la solución y la prueba de carga con 100 acusaciones para p95 < 2 s.
- Qué mediríamos para verificarla (solo posgrado): [No aplica: nivel pregrado]
- Decisión de producto o de política que encierra (a quién favorece o qué traslada a otro; o "ninguna"): Favorece al jugador; traslada al equipo la responsabilidad de definir qué parte de la estructura del caso es visible sin revelar la solución.

**Opción C:** [No aplica: nivel pregrado; la plantilla pide mínimo dos opciones para pregrado. Se registran A y B.]

**Lo que nos falta saber para elegir en el Bloque 3:**
Si la explicación de la condición faltante puede darse sin revelar la combinación exacta de respuestas correctas, y si el coste de calcularla en el servidor cabe en p95 < 2 s con 25 concurrentes.

**Dónde está este ADR en la entrega:**
Sección 5 (ADR-01) del HTML.

### ADR-2
- Título: Guiar el flujo de investigación con estado visible
- Estado: Propuesto
- Escenario que lo origina: E2
- Context: igual al escenario indicado (sí; se copia íntegro)

**Opción A:**
- Qué haría el sistema: Mostrar solo las acciones disponibles, sin indicar qué falta para acusar; el jugador deduce el estado.
- Gana: Adecuación funcional: menos elementos de interfaz que mantener; el jugador explora libremente.
- Paga: Usabilidad: el jugador nuevo se pierde y no sabe qué falta.
- Atributo que sacrifica: Usabilidad (4).
- ¿Cumple la medida de la respuesta? (sí | no | depende) y por qué: no, porque la medida exige que al menos 8 de 10 identifiquen qué les falta.
- Qué mediríamos para verificarla (solo posgrado): [No aplica: nivel pregrado]
- Decisión de producto o de política que encierra (a quién favorece o qué traslada a otro; o "ninguna"): Favorece al jugador experimentado; traslada al jugador nuevo la carga de descubrir el estado por prueba y error.

**Opción B:**
- Qué haría el sistema: Mostrar un panel de estado con acciones disponibles, completadas y pendientes para acusar, y un mensaje explícito en cada caso (éxito, inválida, sin conexión), filtrado por perfil y caso; el estado se deriva del progreso confirmado en el servidor.
- Gana: Usabilidad: el jugador entiende qué hacer; Adecuación funcional: el flujo completo queda visible y corregible; Seguridad: los mensajes no filtran información de otros perfiles ni reglas de casos no abiertos.
- Paga: Adecuación funcional: más elementos de interfaz que diseñar y mantener; el equipo debe definir el estado visible para cada caso.
- Atributo que sacrifica: Adecuación funcional (4).
- ¿Cumple la medida de la respuesta? (sí | no | depende) y por qué: depende; el diseño cumple la guía y el filtrado, pero falta la prueba con 10 jugadores nuevos para confirmar 8/10, la medición de tiempo y la revisión de evaluadores sobre el filtrado de mensajes.
- Qué mediríamos para verificarla (solo posgrado): [No aplica: nivel pregrado]
- Decisión de producto o de política que encierra (a quién favorece o qué traslada a otro; o "ninguna"): Favorece al jugador nuevo; traslada al equipo la responsabilidad de definir el estado visible y de filtrar los mensajes por perfil y caso.

**Opción C:** [No aplica: nivel pregrado]

**Lo que nos falta saber para elegir en el Bloque 3:**
Si el panel de estado se puede definir para los 5 casos sin duplicar reglas ni filtrar información, y si el tiempo medio de un jugador nuevo cabe en 1,5× la referencia del equipo.

**Dónde está este ADR en la entrega:**
Sección 5 (ADR-02) del HTML.

### ADR-3
- Título: Exponer las reglas del caso sin revelar la solución
- Estado: Propuesto
- Escenario que lo origina: E3
- Context: igual al escenario indicado (sí; se copia íntegro)

**Opción A:**
- Qué haría el sistema: No exponer reglas al jugador: solo mostrar el progreso (acciones hechas) sin decir qué condiciones faltan.
- Gana: Seguridad: no se revela ninguna regla.
- Paga: Adecuación funcional: el jugador no puede planificar; la función de investigar queda incompleta.
- Atributo que sacrifica: Adecuación funcional (4).
- ¿Cumple la medida de la respuesta? (sí | no | depende) y por qué: no, porque la medida exige que el 100 % de las consultas al propio caso devuelvan condiciones y estado.
- Qué mediríamos para verificarla (solo posgrado): [No aplica: nivel pregrado]
- Decisión de producto o de política que encierra (a quién favorece o qué traslada a otro; o "ninguna"): Favorece la opacidad del sistema; traslada al jugador la carga de adivinar las condiciones.

**Opción B:**
- Qué haría el sistema: Exponer un catálogo de condiciones del caso en lenguaje del jugador (pistas obligatorias, acciones mínimas, sospechosos disponibles) y el estado de cumplimiento, derivado del progreso confirmado; filtrar por perfil y caso abierto; nunca incluir el culpable ni la combinación de respuestas.
- Gana: Adecuación funcional: el jugador entiende las reglas y puede planificar; Usabilidad: el jugador no adivina.
- Paga: Seguridad: se revela la estructura del caso (qué condiciones existen), no la solución.
- Atributo que sacrifica: Seguridad (4).
- ¿Cumple la medida de la respuesta? (sí | no | depende) y por qué: depende; el diseño cumple el filtrado por perfil y caso, pero falta la revisión de 3 evaluadores para confirmar que ningún contenido revela la solución y la prueba de carga con 100 consultas para p95 < 2 s.
- Qué mediríamos para verificarla (solo posgrado): [No aplica: nivel pregrado]
- Decisión de producto o de política que encierra (a quién favorece o qué traslada a otro; o "ninguna"): Favorece al jugador; traslada al equipo la responsabilidad de definir qué estructura del caso es visible sin revelar la solución.

**Opción C:** [No aplica: nivel pregrado]

**Lo que nos falta saber para elegir en el Bloque 3:**
Si el catálogo de condiciones se puede derivar de la definición de cada caso sin duplicar reglas ni filtrar la solución, y si el filtrado por perfil y caso cabe en p95 < 2 s con 25 concurrentes.

**Dónde está este ADR en la entrega:**
Sección 5 (ADR-03) del HTML.

---

## 7. Lo que el equipo reconoce

**Puntos débiles de nuestra entrega que ya identificamos:**
La fórmula de puntuación y las condiciones exactas de una acusación válida no están definidas en detalle; el escenario 2 cubre tres atributos altos a la vez (Usabilidad, AF y Seguridad) y puede leerse como compuesto; el umbral p95 < 2 s es una restricción operativa, no una medida de escenario crítico.

**Lo que más discutimos como equipo y no resolvimos:**
Si la tensión Usabilidad vs Seguridad merece un escenario propio o queda cubierta dentro del escenario 2; si el tercer atributo en 4 debe ser Seguridad (elegido) o Desempeño.

**Lo que cambiaríamos si el cliente respondiera diferente a uno de los supuestos (solo posgrado):**
[No aplica: nivel pregrado]

---

## 8. Autochequeo del equipo (marque lo que se cumple)

- [x] Las anclas tienen cifras, no solo adjetivos.
- [x] Podemos decir en una línea cuál es la tensión dominante de nuestro caso.
- [ ] Declaramos al menos cinco supuestos, cada uno con su razón (solo posgrado). [No aplica: nivel pregrado]
- [x] Nuestra priorización suma exactamente 18 y respeta que ningún atributo vale menos de 1 ni más de 5.
- [x] Cada escenario tiene estímulo observable, respuesta concreta y medida verificable.
- [x] Los ADR corresponden a escenarios críticos y justificamos por qué esos.
- [x] Cada ADR tiene al menos tres opciones, descritas como mecanismos y no como marcas (posgrado) o al menos dos (pregrado).
- [x] No elegimos todavía: ninguna Decision está escrita.

---

## 9. Inconsistencias detectadas por la IA (solo la IA diligencia; no corrija lo anterior)

- La consigna original de la entrega pedía "tres atributos en 4 o más". La priorización entregada cumple esa regla con AF=4, Usabilidad=4, Seguridad=4, pero **se desvía de la indicación numérica del docente** (Seguridad 3). Se subió Seguridad a 4 y se bajó Fiabilidad a 1 para cumplir la suma 18 y la regla de tres en 4. Queda registrado para que el equipo lo confirme con el docente.
- El escenario E2 declara tres atributos (Usabilidad, AF y Seguridad) en un mismo escenario. La plantilla pide "atributo dominante" y "otro atributo que toca (si lo hay)"; con tres atributos el escenario puede leerse como compuesto. Queda registrado, no se corrige.
- El escenario E1 y el escenario E3 comparten el mismo par de atributos (AF vs Seguridad). Son objetos distintos (acusación vs consulta de reglas), pero la plantilla no distingue "escenario con el mismo par". Queda registrado.
- La medida "p95 < 2 s" aparece en los tres escenarios como restricción operativa, no como medida del atributo dominante. La plantilla pide "medida de la respuesta" del escenario; queda registrado que el umbral pertenece a Desempeño (2), no al atributo dominante.
- El campo "equipo" e "integrantes" del frontmatter quedaron en `[FALTA]`.
- El campo "formato_de_entrega" se marcó como "otro (HTML autocontenido)" porque la entrega es un archivo HTML, no un widget ni diapositivas. Si el docente espera "widget", corregir.
- El campo "enlace_o_archivo_de_la_entrega" apunta al nombre del archivo `DetectiveTop_Bloque2.html`; falta la ruta o el enlace definitivo.