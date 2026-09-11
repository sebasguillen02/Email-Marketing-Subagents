# Auditoría técnica — Programa de Email Marketing DOU Foods (Klaviyo + WooCommerce)

**Analista:** Senior Email Marketing Analyst (10+ años, Klaviyo/D2C)
**Fecha:** 11 de septiembre de 2026
**Alcance:** Fase 1 (Flujos) + Fase 2 (Campañas y Newsletter)

---

## Resumen ejecutivo

La estrategia tiene una base sólida de **copywriting y tono de marca** — voz consistente, argentina, con humor y sin caer en genérico. El uso de DOU coins como hilo narrativo transversal está bien pensado. Pero como programa de Klaviyo evaluado con estándares de ecommerce serio, está **incompleto en automatizaciones críticas de revenue** (Browse Abandonment, Winback/Sunset, Upsell), **no usa la capa de segmentación y personalización predictiva que Klaviyo ofrece nativamente**, y **no tiene ningún framework de medición ni política de deliverability**. Es una base de "brand voice" sobre la que falta construir la infraestructura técnica de un programa de email que genere 30-40% de la revenue total, que es el benchmark esperado en ecommerce con Klaviyo bien implementado.

Calificación global: **6/10** — buen copy, arquitectura de flujos incompleta, cero framework de datos/testing/deliverability.

---

## 1. Arquitectura de flujos: triggers, timing y condiciones

### Lo que funciona
- **Flujo de Bienvenida**: el filtro "No compró nunca" es correcto y evita que clientes existentes reciban un flujo de adquisición. El trigger por lista es estándar.
- **Carrito Abandonado**: usar `Started Checkout` como trigger y el bloque de **Dynamic Checkout Content** es la implementación nativa correcta de Klaviyo-WooCommerce. Está bien documentada la limitación real de WooCommerce (solo captura emails ingresados en checkout).
- **Post-Compra**: el trigger `Placed Order` con confirmación inmediata es correcto, y la referencia a WhatsApp en el footer es una buena práctica de servicio al cliente en LATAM.
- **Flujo de Recompra**: el timing de 21/35 días está basado en un dato real de consumo (caja dura 2-3 semanas), lo cual es más riguroso que copiar un benchmark genérico.

### Lo que falla o está incompleto

1. **Bienvenida con solo 2 emails es corto.** El benchmark de Klaviyo para welcome series de ecommerce es de 4 a 7 emails distribuidos en 2-3 semanas. Faltan emails de: prueba social/UGC (reseñas, fotos de clientes), FAQ/objeciones (envío, vencimiento, ingredientes), y un email final con incentivo monetario real (no solo DOU coins) para el segmento que llegó al día 7-10 sin comprar. Actualmente el flujo "muere" a los 3 días sin abrir una segunda instancia de conversión.

2. **Falta un Conditional Split para gente que abre/clickea vs. no.** Klaviyo permite bifurcar el flujo de bienvenida según engagement (`Opened Email` / `Clicked Email`) para enviar contenido distinto a quien ya mostró interés (oferta más agresiva) vs. quien ni siquiera abrió (probar otro subject o canal). No está contemplado en ningún flujo.

3. **No hay salida cruzada entre flujos.** Si un contacto en el Flujo de Bienvenida agrega un producto al carrito y abandona, ¿lo saca del welcome flow para entrar al de Carrito Abandonado, o recibe ambos en paralelo? Esto se resuelve con **Flow Filters** (excluir por "Started Checkout since starting flow") y no está mencionado en absoluto. Riesgo real de mensajes contradictorios el mismo día.

4. **Carrito Abandonado usa solo `Started Checkout`, ignorando `Added to Cart`.** La integración de Klaviyo con WooCommerce dispara ambos eventos por separado. Mucha gente agrega al carrito y nunca llega a iniciar checkout — ese segmento no está cubierto por ningún flujo. Se debería tener un flujo (o una rama condicional dentro del mismo) para `Added to Cart` sin `Started Checkout`, típicamente con un mensaje más suave (no "tu pedido" sino "dejaste algo en el carrito").

5. **Cero Browse Abandonment.** Ver punto 6 en detalle — es la ausencia más grave de toda la arquitectura.

6. **Post-Compra no tiene rama por tipo de cliente.** Debería existir un **Conditional Split "Placed Order Count = 1" vs. "> 1"** para diferenciar el copy de primera compra (más educativo, "así se come un DOU") del de cliente recurrente (más directo, foco en coins/tier). Actualmente todos reciben el mismo email 3.1 y 3.2.

7. **No hay email de shipping/tracking separado.** El flujo de Post-Compra menciona "te mandamos el número de seguimiento cuando sale" pero no está definido como un email propio disparado por un evento de fulfillment (ej. metric personalizado `Order Fulfilled` sincronizado desde WooCommerce/plugin de shipping). Esto es un touchpoint de alto open rate (la gente quiere saber dónde está su pedido) que se está desperdiciando como línea de texto en el email de confirmación.

8. **Flujo de Recompra se corta en el día 35.** No hay continuidad hacia un Winback real (60-90-120 días) ni hacia un Sunset flow. Ver puntos 6 y 9.

9. **Cumpleaños: falta el pre-día y falta la captura de fecha como flujo propio.** El documento asume que ya se recolectó la fecha ("requiere recolectar fecha... campo en formulario o campaña dedicada") pero no diseña ese flujo de progressive profiling (ej. un email post-compra +14 días pidiendo fecha de nacimiento a cambio de coins). Sin ese flujo, el Flujo 5 nunca va a tener cobertura de lista significativa.

---

## 2. Subject lines y preview text: potencial de apertura y metodología de A/B testing

### Lo que funciona
- Los subjects son coloquiales, cortos, sin caer en clickbait forzado ("Ya sos parte de la DOU Gang", "Team NOM, XD o NT?"). Buen uso de curiosidad y segunda persona.
- El preview text está tratado como copy propio (no repite el subject), lo cual muchos programas de Klaviyo ignoran.

### Lo que falla

1. **No hay metodología de A/B testing definida.** El documento propone variantes A/B en casi todos los emails, pero en ningún lado se especifica:
   - Tamaño de muestra / % de audiencia por variante antes de enviar el ganador al resto (Klaviyo permite definir esto explícitamente en el paso de "Send Time" del test).
   - Métrica de decisión del ganador: si se elige por **Open Rate**, hoy es una métrica **contaminada por Apple Mail Privacy Protection (MPP)**, que infla aperturas artificialmente (bots de Apple pre-cargan el email). El estándar 2024-2026 es decidir ganadores por **Click Rate** o, mejor aún, **Revenue per Recipient (RPR)**.
   - Duración del test / tiempo de espera antes de declarar ganador.
   - Significancia estadística mínima. Con una lista chica (típica de una marca D2C en etapa de crecimiento en Argentina), es probable que muchos de estos tests de flujo nunca alcancen n suficiente para ser concluyentes — hay que priorizar testear en **campañas** (mayor volumen por envío) y no diluir tests en flujos de bajo volumen.

2. **Solo se testea el subject line, nunca el preview text como combinación, ni el remitente (from name), ni el horario de envío.** Klaviyo permite testear estos elementos de forma nativa. El From Name no se menciona en ningún lugar del documento — es tan determinante para el open rate como el subject.

3. **No se usa Smart Send Time / Send Time Optimization (STO) de Klaviyo.** Todos los envíos son a hora fija (ej. newsletter "primer lunes", campañas a fecha fija). Klaviyo puede optimizar hora de envío por perfil individual en campañas, algo que no está contemplado ni para testear ni para producción.

4. **Riesgo de spam en subjects de urgencia extrema.** "Última chance", "se va en 24hs", "Black Friday. Solo hoy." son válidos en frecuencia moderada, pero si se acumulan (como pasa en el calendario de noviembre-diciembre, ver punto 7) el patrón agregado de urgencia constante puede erosionar la confianza del suscriptor y elevar spam complaints — no hay ningún control mencionado sobre esto.

---

## 3. Copy y tono: potencial de conversión, claridad y efectividad de CTAs

### Lo que funciona
- La voz de marca es distintiva y consistente: irreverente pero no forzada, con guiños locales bien calibrados para el público objetivo.
- Buena jerarquía de CTA en el email de reseña (3.2): la reseña es el CTA principal y el referido va como P.D., evitando el error clásico de "doble ask" que diluye conversión. Esto demuestra criterio profesional.
- La ética alrededor de la escasez ("Dato: el NT se está agotando — Solo usar si es real") es correcta y debería aplicarse como regla transversal a todo el programa, no solo a ese email.

### Lo que falla
1. **CTAs genéricos y repetidos.** "Elegí tu favorito", "Completar mi pedido", "Ver mi tienda" aparecen sin variación relevante entre flujos. Los CTAs de mejor performance en ecommerce son específicos al beneficio o en primera persona ("Quiero mi DOU", "Sí, quiero mi caja"), y deberían variarse por test A/B tanto como los subjects — cosa que no se propone en ningún lado.
2. **Ausencia total de prueba social dentro del copy de los emails.** No hay bloques de reseñas, rating con estrellas, ni contenido generado por usuarios (fotos de Instagram) en ningún flujo, ni siquiera en Post-Compra o Bienvenida donde más impacto tiene en la decisión de un comprador nuevo. Klaviyo se integra nativamente con Klaviyo Reviews, Yotpo o Judge.me para insertar bloques dinámicos de rating — no se menciona ninguna herramienta de reviews.
3. **No hay manejo de reseñas negativas.** El flujo 3.2 pide reseña sin ninguna bifurcación: si el feedback es negativo (ej. captura de estrellas 1-2 antes de redirigir a la plataforma pública), debería derivar a un flujo de recuperación de cliente / contacto de soporte en vez de empujar la reseña pública. Tal como está diseñado, un cliente insatisfecho puede terminar dejando una reseña negativa pública que la marca podría haber interceptado.
4. **Diseño mobile-first no está mencionado.** Con 60-70% de aperturas en mobile en LATAM, no hay ninguna nota sobre jerarquía visual, tamaño de botones táctiles, o testing en clientes de correo mobile.

---

## 4. Implementación técnica: features de Klaviyo usados y ausentes

### Usados correctamente
- **Dynamic Checkout Content** (carrito abandonado) — correcto y bien nombrado.
- Personalización básica con `{{first_name}}`.
- Trigger nativo `Placed Order` de la integración WooCommerce.

### Ausentes — esto es el corazón del problema técnico

- **Conditional Splits / branching lógico**: no aparece en ningún flujo. Es la herramienta que separa un programa amateur de uno profesional en Klaviyo. Aplicaciones concretas que faltan:
  - Bienvenida: split por engagement temprano.
  - Carrito abandonado: split por **valor del carrito** (carritos de alto valor → oferta más agresiva o notificación interna al equipo).
  - Post-compra: split por primera compra vs. recurrente, y por sabor comprado (para cross-sell dirigido).
  - Recompra: split por sabor histórico para recomendar el complementario (quien compró NOM recibe sugerencia de probar NT).

- **Segmentación de campañas**: no hay ninguna mención de segmentar las campañas de Fase 2. Todo indica envío a lista completa. Esto es grave porque:
  - No hay segmento de **"Engaged 90 días"** para proteger deliverability en campañas de alto volumen (CyberMonday, Black Friday, Navidad).
  - No hay segmento **VIP / alto CLV** para dar acceso anticipado real (aunque el lanzamiento de producto lo simula con "los de la lista se enteran primero", no hay una capa VIP dentro de la lista misma).

- **Predictive Analytics de Klaviyo** (requiere historial de `Placed Order` vía la integración WooCommerce): no se usa **Predicted Next Order Date** ni **Customer Lifetime Value** ni **Churn Risk**, cuando estas son exactamente las señales que deberían fijar el timing del Flujo de Recompra en vez de un valor fijo de 21/35 días para todos los perfiles. Un cliente que históricamente recompra cada 15 días no debería esperar al día 21 para el primer recordatorio.

- **Catalog / Product Feed dinámico**: la newsletter menciona "EL DOU DEL MES" como bloque rotativo, pero no se especifica si usa el Product Block dinámico de Klaviyo conectado al feed de WooCommerce (que permite recomendaciones dinámicas tipo "más vendidos" o "based on last purchase") o si es swap manual mes a mes. Si es manual, es un desperdicio de una función nativa.

- **Custom Properties / Metrics para personalización de sabor**: se menciona como "mejora futura" en el Flujo de Recompra, pero debería ser un requisito desde el día 1 — es un custom property trivial de sincronizar (`Last Purchased Flavor`) vía la propiedad de line items del pedido, y habilita personalización en absolutamente todos los flujos, no solo en uno.

- **SMS**: no se menciona en ningún punto del documento. Klaviyo es una plataforma omnicanal y el combo Email+SMS en carrito abandonado y flash sales (CyberMonday/Black Friday) típicamente sube la tasa de recuperación 20-30% adicional. Puede ser una decisión consciente por costo/regulación en Argentina, pero el documento no lo aborda ni para descartarlo con criterio.

- **Smart Sending / Flow-Campaign suppression**: con 5 flujos activos + newsletter mensual + calendario cargado de campañas estacionales (ver punto 7), no hay ninguna mención de la función de Klaviyo que evita que un mismo perfil reciba más de X emails en Y horas, ni de exclusión mutua entre flujos y campañas activas el mismo día.

---

## 5. Estrategia de deliverability: lo que hay y lo que falta

### Lo que hay (parcial, y bien ejecutado donde aparece)
- **Reply-to como señal de engagement**: aparece en el Carrito Abandonado (2.2) y en Recompra (4.2), pidiendo "respondé este mail". Es una práctica correcta — Gmail/Outlook usan las respuestas como señal positiva fuerte para inbox placement. Buen instinto, pero está aplicado de forma ad-hoc en solo 2 de más de 15 emails, no como política consistente.

### Lo que falta — y es la brecha más riesgosa de todo el programa
1. **No hay Sunset Flow / política de higiene de lista.** No existe ningún mecanismo para dejar de enviarle a contactos que no abren ni clickean hace 90-180 días. Sin esto, la lista acumula "peso muerto" que ISPs como Gmail penalizan con menor inbox placement para **todos** los envíos, incluidos los flujos que sí funcionan.
2. **No hay definición de segmento "Engaged"** para filtrar a quién se le manda campañas masivas (newsletter, CyberMonday, Black Friday, Navidad). Mandarle a la lista completa sin filtrar por engagement reciente es la causa número uno de degradación de sender score en cuentas de Klaviyo de marcas chicas/medianas.
3. **Cero mención de autenticación de dominio** (SPF, DKIM, DMARC) ni de dominio de envío dedicado — básico pero crítico, y no puede asumirse resuelto sin verificarlo explícitamente en la configuración de Klaviyo.
4. **No hay plan de manejo de bounces/spam complaints** (umbral de alerta, remoción automática).
5. **No se menciona política de opt-in del pop-up** (single vs. double opt-in), relevante en LATAM donde la calidad de la captura de pop-ups suele ser baja y sin higiene genera list rot desde el día 1.
6. **El calendario de Fase 2 (nov-dic) apila ~9-11 campañas en 10 semanas** sin ningún control de frecuencia — esto es una bomba de tiempo para deliverability si no se segmenta por engagement (ver punto 7).

---

## 6. Carrito Abandonado: ¿alcanza con 2 emails? ¿Falta Browse Abandonment?

**No, 2 emails no alcanzan.** El benchmark de Klaviyo para ecommerce es de **3 emails** en la secuencia de carrito abandonado. La estructura actual (1h, 24h) cubre la recuperación rápida pero abandona al usuario justo cuando más necesita un empujón adicional: un tercer email a las **48-72 horas** con un incentivo concreto (no solo DOU coins, sino un % de descuento o envío gratis sin mínimo) suele recuperar un 15-25% adicional de la revenue de la secuencia completa. Hoy el programa nunca ofrece descuento monetario en ningún punto de la secuencia de recuperación — solo coins y envío gratis condicionado a $40.000, lo cual puede no ser suficiente motivador para quien abandonó por precio.

**Browse Abandonment: ausencia total y es la falla más costosa de todo el documento.** No existe ningún flujo disparado por el evento `Viewed Product` (que Klaviyo captura automáticamente vía snippet en WooCommerce) para usuarios que navegaron una página de producto sin agregar al carrito. En cualquier ecommerce, el volumen de "Viewed Product" es varias veces mayor al de "Started Checkout" — es decir, hay muchísimo más tráfico cualificado sin ningún touchpoint de recuperación que en el carrito abandonado. Se recomienda un flujo de 2 emails (4h y 24h post-vista) con el producto visto en Dynamic Content, mensaje más suave que el de carrito ("¿Te quedaste pensando en el NT?") sin urgencia de "pedido incompleto" porque no hubo intención de compra explícita.

---

## 7. Calendario de campañas: riesgo de frecuencia, horario de envío y segmentación

El calendario de Fase 2 acumula, solo entre septiembre y diciembre:
Newsletter mensual (x3-4) + Día del Estudiante + Día de la Madre + CyberMonday (x2) + Lanzamiento de producto (x3) + Black Friday + Navidad (x2) + Cierre de temporada = **entre 12 y 14 campañas** en ~14 semanas, superpuestas con los 5 flujos always-on.

Problemas concretos:
1. **No hay segmentación por engagement para ninguna de estas campañas.** Deberían excluirse los no-engaged de 90+ días de los envíos de alto volumen (CyberMonday, Black Friday) para proteger el sender score justo en la temporada de mayor revenue del año — el peor momento para tener problemas de deliverability.
2. **No hay frequency capping.** Si un contacto está en el Flujo de Recompra (día 21 o 35) el mismo día que sale una campaña de CyberMonday, puede recibir 2 emails el mismo día sin ningún control. Klaviyo permite fijar un máximo de emails por perfil en X días — no está mencionado.
3. **Horarios fijos sin optimización.** "Primer lunes del mes", "19 de septiembre", "2 de noviembre" — todas son fechas de calendario, no hay ninguna capa de Send Time Optimization por perfil ni testing de horario (mañana vs. tarde vs. noche) para maximizar aperturas.
4. **Sin priorización de audiencia en el lanzamiento de producto** (la campaña de mayor prioridad según el propio documento): no hay un segmento VIP que reciba el "acceso anticipado" con un beneficio real y diferenciado (ej. 24-48h antes que el resto de la lista, o unidades reservadas) — actualmente "acceso anticipado" es el mismo mensaje para toda la lista, lo que le quita fuerza al concepto de exclusividad que el propio copy promete.

---

## 8. Programa de lealtad (DOU coins): ¿está bien aprovechado en los emails?

Está **mencionado de forma consistente** (aparece en Bienvenida, Carrito Abandonado, Post-Compra, Recompra, Cumpleaños, Newsletter) lo cual es positivo como refuerzo narrativo. Pero el aprovechamiento es **superficial**: se usa como frase de refuerzo, nunca como **dato estructurado que dispare personalización o segmentación**.

Lo que falta:
1. **No hay flujo disparado por saldo de coins** (ej. "Te faltan 50 coins para tu próxima caja gratis" cuando el perfil cruza cierto umbral) — esto requiere sincronizar el saldo de coins como custom property del perfil en Klaviyo, algo que el documento nunca contempla técnicamente.
2. **No hay segmentación por tier/nivel de coins** para dar mensajes diferenciados a los "top spenders" (ej. acceso anticipado real al lanzamiento, como se señaló en el punto 7) versus el resto de la lista.
3. **No hay campaña de coins por vencer**, si el programa tiene expiración de puntos — dato crítico que no se aclara pero que, de existir, es una de las campañas de mayor CTR típicas en programas de loyalty.
4. **El referido está subutilizado como mecanismo de captación**: aparece solo como P.D. en un email (correcto para no competir con el CTA principal de reseña) pero no existe un flujo propio disparado por generación/uso de código de referido, que permitiría automatizar el agradecimiento y el refuerzo de las 300 coins ganadas.
5. **El "ranking" de la newsletter (TOP 5 DOU Gang) es un excelente gancho de gamificación** pero no se conecta con ninguna campaña de urgencia tipo "te quedan 3 días para entrar al Top 5 del mes" dirigida específicamente a quienes están cerca del corte — eso requeriría exponer el ranking como dato segmentable en Klaviyo, no solo como contenido de newsletter.

---

## 9. Automatizaciones faltantes que todo programa serio de Klaviyo debería tener

En orden de impacto esperado:

1. **Browse Abandonment** (ver punto 6) — la ausencia más costosa.
2. **Winback / Reactivación de largo plazo** (60-90-120+ días sin compra), distinto del Flujo 4 de Recompra que se corta a los 35 días. Debería incluir oferta creciente en agresividad e idealmente cerrar con una encuesta de salida.
3. **Sunset / Suppression flow** para contactos no-engaged de 180+ días — reduce el envío a perfiles muertos antes de que dañen el sender score, con un último intento de reactivación tipo "¿seguís queriendo saber de nosotros?" antes de suprimir.
4. **Post-Purchase Cross-sell/Upsell** (30-45 días post-compra, independiente del flujo de recompra del mismo producto) — sugerir el sabor complementario o el combo MIX a quien compró un sabor único.
5. **Flujo VIP / por tier de cliente** (basado en CLV predictivo o cantidad de compras) con beneficios exclusivos, no solo mensajes.
6. **Back in Stock** — si el NT u otro sabor se agota (mencionado como riesgo real en el propio documento: "el NT se está agotando"), no hay ningún flujo automático de notificación de reposición.
7. **Formulario abandonado** — Klaviyo puede capturar el email en el momento en que se tipea en el pop-up aunque no se envíe el formulario completo; no se usa.
8. **Aniversario de primera compra** — touchpoint de bajo costo y alto valor percibido, ausente.
9. **Encuesta de feedback para no-compradores del welcome flow** que llegaron al final sin comprar (por qué no compraron), distinta de la que ya existe para quienes dejan de comprar en Recompra.
10. **Recordatorio de coins por vencer** (si aplica vencimiento).

---

## 10. Framework de métricas: qué deberían medir y cómo

El documento **no define ningún KPI, objetivo numérico, ni proceso de revisión**. Esto es una falla estructural: sin métricas no hay forma de saber si el programa funciona ni de priorizar qué arreglar primero. Se recomienda:

**Métricas primarias (nivel programa)**
- **Revenue per Recipient (RPR)** por flujo y por campaña — no Open Rate, que está distorsionado por Apple MPP desde 2021. Usar RPR y Click Rate como métricas de decisión de tests A/B.
- **% de revenue total de ecommerce atribuible a email** (flujos + campañas) — benchmark sano en D2C con Klaviyo maduro: 30-40%. Si hoy está por debajo del 20%, el gap está mayormente en las automatizaciones faltantes del punto 9.
- **% de revenue de flujos vs. campañas** — los flujos deberían representar 60-70% de la revenue de email porque están siempre activos y altamente segmentados; si las campañas dominan, es señal de que los flujos están subdesarrollados (que es el caso actual).

**Métricas de flujo específicas**
- Conversion rate por email individual dentro de cada flujo (no solo del flujo agregado) — para saber si el email 1.1 o el 1.2 es el que realmente convierte en Bienvenida.
- Tasa de recuperación del Carrito Abandonado (% de `Started Checkout` que termina en `Placed Order` dentro de la ventana del flujo) — benchmark de referencia 3-10% según industria.
- Días promedio entre compras (para calibrar el timing 21/35 del Flujo de Recompra contra el dato real, y eventualmente reemplazarlo por Predicted Next Order Date).

**Métricas de deliverability (deberían revisarse cada campaña, no una vez al año)**
- Bounce rate (objetivo <2%), Spam complaint rate (objetivo <0.1%), Unsubscribe rate por envío (<0.5%).
- Tamaño y % de la lista clasificado en el segmento "Engaged 90 días" — si cae, es la primera alarma antes de que baje el inbox placement.

**Métricas de testing**
- Un log de tests A/B con: hipótesis, variantes, métrica de decisión, tamaño de muestra, resultado, y si se implementó como default. Hoy los tests se proponen ad-hoc en cada email sin acumular aprendizaje entre uno y otro.

**Métricas de loyalty**
- Tasa de canje de DOU coins (engagement real con el programa, no solo acumulación).
- % de segunda compra atribuible a mención de coins en el copy (requiere UTM/tracking de click por bloque).

---

## TOP 5 ACCIONES MÁS URGENTES (ordenadas por impacto esperado en revenue y engagement)

### 1. Construir Browse Abandonment y extender Carrito Abandonado a 3 emails con incentivo monetario real
**Por qué es la #1:** es la brecha de mayor volumen de tráfico cualificado sin ningún touchpoint de recuperación (Viewed Product > Started Checkout en cualquier ecommerce). Sumado a esto, agregar un tercer email de Carrito Abandonado a las 48-72h con un descuento concreto (no solo coins/envío gratis) recupera típicamente 15-25% adicional sobre la secuencia actual.
**Acción técnica:** activar el flujo basado en el evento `Viewed Product` (2 emails, 4h/24h, con Dynamic Content del producto visto) + agregar el email 2.3 al flujo de Carrito Abandonado con oferta de descuento definida junto al equipo de pricing.
**Métrica de éxito:** revenue recuperada por flujo (RPR) medida a 30 días de implementación, y % de `Started Checkout` convertido a `Placed Order`.

### 2. Implementar segmento "Engaged" y Sunset/Winback flow antes de la temporada alta de noviembre-diciembre
**Por qué es urgente:** el calendario de Fase 2 apila 12-14 campañas en 14 semanas justo cuando el programa no tiene ningún filtro de deliverability. Sin esto, se arriesga la caída del inbox placement en el momento de mayor revenue potencial del año (CyberMonday + Black Friday + Navidad + lanzamiento de producto), lo cual afecta a **todos** los envíos, no solo a las campañas nuevas.
**Acción técnica:** crear segmento Klaviyo "Engaged 90 días" (abrió o clickeó en los últimos 90 días) y usarlo como audiencia base de toda campaña masiva desde ya; construir un Winback flow (60-90-120 días) y un Sunset flow (180+ días sin engagement) antes de octubre.
**Métrica de éxito:** bounce rate <2%, spam complaint <0.1%, tamaño del segmento Engaged estable o creciente mes a mes.

### 3. Segmentar todas las campañas de Fase 2 y activar frequency capping / Smart Sending
**Por qué es urgente:** hoy no hay evidencia de que ninguna campaña se segmente, y el calendario de nov-dic se superpone con los flujos always-on sin ningún control de frecuencia por perfil — riesgo directo de fatiga de lista justo en la ventana de mayor presión comercial.
**Acción técnica:** definir al menos 3 segmentos de campaña (Engaged, VIP/alto CLV, No compradores recientes) y aplicar límite de frecuencia máxima por perfil en Klaviyo (ej. no más de 1 email de campaña + 1 de flujo por día); dar acceso anticipado real (no simbólico) al segmento VIP en el lanzamiento de producto de noviembre.
**Métrica de éxito:** unsubscribe rate por campaña, CTR segmentado vs. lista completa, revenue por segmento VIP vs. resto.

### 4. Agregar Cross-sell/Upsell post-compra y personalizar el timing del Flujo de Recompra con Predicted Next Order Date
**Por qué es urgente:** el Flujo de Recompra usa un timing fijo (21/35 días) para toda la base, ignorando la data de compra individual que Klaviyo ya tiene disponible vía la integración de WooCommerce. Además, no existe ningún flujo que empuje la compra de un segundo sabor a quien ya compró uno solo — el cross-sell más obvio del catálogo (3 sabores + MIX) no está automatizado.
**Acción técnica:** sincronizar `Last Purchased Flavor` como custom property; construir un Conditional Split en Post-Compra que recomiende el sabor complementario a los 30-45 días; migrar el trigger del Flujo de Recompra de "días fijos" a un rango basado en `Predicted Next Order Date` cuando haya suficiente historial de pedidos.
**Métrica de éxito:** % de clientes con más de un sabor en su historial de compra, revenue incremental del flujo de cross-sell, reducción del tiempo entre 1ra y 2da compra.

### 5. Establecer un framework de métricas y testing formal (RPR como KPI primario, log de tests, objetivo de % de revenue por email)
**Por qué es urgente:** sin esto, ninguna de las mejoras anteriores puede validarse ni priorizarse con datos, y las decisiones de A/B testing (que hoy se basan implícitamente en Open Rate) están usando una métrica distorsionada por Apple Mail Privacy Protection desde 2021.
**Acción técnica:** fijar Revenue per Recipient y Click Rate como métrica de decisión en todos los tests A/B de Klaviyo (nunca Open Rate); definir objetivo de % de revenue de ecommerce atribuible a email (benchmark 30-40%) y de % de esa revenue proveniente de flujos vs. campañas (objetivo 60-70% flujos); crear un log de tests con hipótesis/resultado para acumular aprendizaje entre campañas en vez de testear de forma aislada en cada envío.
**Métrica de éxito:** % revenue de email sobre revenue total de ecommerce medido mensualmente, tasa de tests A/B con resultado estadísticamente significativo.

---

*Fin del análisis.*
