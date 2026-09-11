# DOU Foods — Auditoría de estrategia de Email Marketing (visión CMO)

**Fecha:** 11 de septiembre de 2026
**Alcance:** Klaviyo + WooCommerce — Fase 1 (flujos automáticos) y Fase 2 (campañas/newsletter)

---

## Veredicto general

Hay un equipo con buen instinto de copywriting y sentido común de flujos básicos (welcome, carrito, post-compra, recompra, cumpleaños). El tono está bien logrado y es consistente en el 90% de los emails. Pero esto es un **plan de contenidos de email**, no una **estrategia de revenue**. Faltan las piezas que hacen que un programa de email pase de "genera algunos pesos" a "es el segundo canal de revenue después de paid/orgánico": suscripción, segmentación real, arquitectura anti-fatiga, economía de loyalty explícita y métricas. Lo desarrollo punto por punto.

---

## 1. Posicionamiento de marca y consistencia de voz

**Lo que funciona:** la voz "DOU Gang" —informal, con humor porteño, "vos", frases cortas, guiños tipo "no es joda"— está bien sostenida a lo largo de casi todos los flujos. Es una marca con personalidad, no un ecommerce genérico de alfajores. Eso vale mucho en un mercado (alfajores premium D2C) donde la mayoría compite por precio o nostalgia.

**Lo que falla:**

- **Ambigüedad de audiencia no resuelta.** El Email 1.1 propone un test A/B con la variante "DOU es el alfajor que tu hijo no va a dejar de pedirte", que apunta a un padre/madre comprando para un chico. Todo el resto del programa (DOU Gang, coins, "Team NOM/XD/NT", ranking mensual, Benja) está construido para un consumidor joven que se compra el alfajor para sí mismo. Son dos brand voices distintas conviviendo sin segmentación explícita. Si el test gana en la variante "padres", ¿qué pasa con el resto del customer journey de ese usuario? ¿Sigue recibiendo "Team NOM, XD o NT?" tres días después? No hay bifurcación de journey por esta señal. Esto no es un detalle: define si DOU es una marca de indulgencia personal (más parecido a Rappi/snacks) o un producto para "comprar para otro" (más parecido a regalo/gifting). Son dos modelos de negocio distintos con distinto AOV, distinta frecuencia y distinto mensaje.

- **"Benja" no está presentado.** Se menciona "un saludo de Benja" y "video de Benja" como si fuera un personaje conocido, pero un suscriptor nuevo en el flujo de bienvenida no tiene ningún contexto de quién es. Si Benja es el founder o la cara de la marca, merece un párrafo de introducción en algún punto temprano del journey (About/Founder story), si no, el "premio" pierde todo su valor percibido.

- **Tono parejo para todas las ocasiones, incluso cuando no corresponde.** El mismo registro irreverente que funciona en Black Friday ("sin vueltas, sin cuenta regresiva eterna") se aplica también a Día de la Madre con un ángulo "práctico, no emotivo" que es una decisión de riesgo: para un producto de regalo emocional, posicionarse como "la salida fácil" puede leerse como poco cuidado, especialmente si tu comprador es alguien de 35-55 años, no el target Gen Z/millennial de la DOU Gang. La propia estrategia ya lo admite en las notas ("posiciona a DOU como salida fácil, no como regalo soñado") pero no propone una alternativa premium para quien sí quiere emocionalidad en el regalo (ej. mensaje personalizado en la caja, tarjeta).

- **Uso de "2026"/"2027" en el cierre de temporada:** revisar que la fecha del documento (temporada 2026, cierre en diciembre) sea coherente con el resto del calendario (Black Friday 27/11, CyberMonday 3-5/11 — esas fechas son de 2026, ok, consistente).

---

## 2. Potencial de revenue y estrategia de CLV

Este es el punto más débil del documento completo.

- **No hay modelo de suscripción.** DOU vende un producto de consumo recurrente (una caja dura 2-3 semanas, textualmente lo dice el propio Flujo 4). Es el caso de uso de manual para un programa "suscribite y ahorrá 10-15%, elegí tu frecuencia". En lugar de eso, tienen un flujo de re-conquista manual a los 21 y 35 días que depende de que el cliente vuelva a decidir activamente. Eso es dejar sobre la mesa el mecanismo de mayor impacto en CLV que existe para este tipo de producto. Una suscripción con revenue predecible cambia toda la conversación de "cuántos emails de recompra mando" a "cuántos activo en el plan".

- **No hay upsell/cross-sell de AOV.** El envío gratis a partir de $40.000 se menciona en casi todos los emails, pero nunca como mecanismo activo de aumento de ticket (no hay barra de progreso "te faltan $X para envío gratis" en el carrito abandonado, que es el lugar exacto donde más convierte). Tampoco hay bundles más grandes (caja de 24, pack "6 meses"), ni cross-sell basado en el sabor comprado (mencionado solo como "mejora futura").

- **Cero mención de regalos corporativos / B2B.** Los alfajores son uno de los productos de regalo empresarial más fuertes en Argentina (fin de año, día del empleado, eventos). Es una línea de revenue de alto ticket promedio y alto margen (compra en volumen, sin descuento agresivo) completamente ausente del plan.

- **La economía de DOU coins nunca se cuantifica.** Se mencionan constantemente ("sumás DOU coins", "tus coins siguen ahí") pero en ningún lugar del documento se explicita cuántos coins equivalen a cuánto gasto, ni el costo real para el negocio de cada canje. Sin esa definición, no se puede modelar el impacto en margen ni en LTV — es una promesa de loyalty sin motor financiero visible detrás.

- **No hay segmentación por valor de cliente.** Todo el journey trata igual a alguien que compró una vez por $19.990 y a alguien que ya lleva 6 compras y $150.000 acumulados. No existe un programa VIP, ni trato diferencial, ni un umbral de "cliente de alto valor" con journey propio.

---

## 3. Arquitectura de funnel: gaps del customer journey

Los 5 flujos cubren el journey clásico "top of funnel a retención básica", pero faltan piezas estándar en cualquier programa maduro de Klaviyo:

- **Browse Abandonment**: no existe. Solo hay Started Checkout (limitado, como el propio documento reconoce, a quien ya dejó el email en checkout). Klaviyo puede trackear "Viewed Product" vía su script de sitio sin depender de WooCommerce checkout — esto es un flujo estándar de altísimo ROI que falta completamente.
- **Post-Purchase Cross-Sell**: el flujo de post-compra va directo a reseña y coins; no hay ningún email de upsell durante la ventana de espera del envío (24-48hs) que sugiera producto complementario o la suscripción.
- **Winback profundo (90+ días)**: el Flujo 4 llega hasta el día 35. No hay tratamiento para clientes con 90, 120, 180 días de inactividad, que necesitan una oferta distinta (más agresiva, o una encuesta de "por qué te fuiste") antes de darlos por perdidos.
- **Sunset / re-permission flow**: no hay ningún mecanismo para limpiar la base de contactos que nunca abren (crítico para deliverability, ver punto 8).
- **Preference Center**: no existe manera de que el suscriptor elija frecuencia o sabor preferido — toda la personalización queda en "mejora futura".
- **Zero-party data collection**: no hay quiz ni encuesta de onboarding ("qué sabor te gusta más", "cómo nos conociste", "para vos o para regalar") que alimente segmentación real desde el día 1.
- **WhatsApp/SMS no están integrados al journey de marketing**, solo aparecen como canal de atención al cliente. En Argentina, con altísima penetración de WhatsApp, dejar ese canal fuera de la estrategia de recompra/carrito abandonado es una oportunidad perdida grande.

---

## 4. Calendario de campañas: timing, frecuencia, riesgo de fatiga

Acá hay una bomba de tiempo mal diseñada: **noviembre está sobrecargado.**

Contando solo campañas (sin sumar los flujos automáticos que siguen corriendo en paralelo sobre la misma base):
- Teaser lanzamiento (7-10 días antes)
- Reveal lanzamiento (2-3 días antes)
- CyberMonday email 1 (2 nov)
- CyberMonday email 2 (5 nov)
- Lanzamiento día D (posiblemente fusionado con CyberMonday)
- Black Friday (27 nov)

Eso son entre 5 y 6 emails de campaña en un mes, más el newsletter mensual, más quien esté en flujo de recompra, cumpleaños o carrito abandonado en simultáneo. Un cliente activo puede recibir 8-10 emails de DOU en 30 días. No hay ninguna regla de supresión ni frequency capping mencionada (ej. "si ya recibió 3 emails de campaña esta semana, no entra a X flujo"). Esto es la receta clásica para pico de bajas de suscripción y caída de engagement justo en el mes de mayor oportunidad de revenue del año (Black Friday + CyberMonday + lanzamiento).

Otros puntos de calendario:
- El newsletter "solo se manda si hay contenido" es razonable para calidad, pero para un programa maduro genera cadencia irregular, lo cual perjudica la reputación de envío a largo plazo (Gmail/Outlook penalizan patrones de envío inconsistentes).
- No hay send-time optimization ni test de horario/día mencionado en ningún punto.
- Diciembre, en cambio, está bastante liviano (solo 2 campañas + cierre de temporada) para ser temporada alta de regalos — ahí sobra espacio para una campaña adicional de "últimos días" tipo gift guide segmentada, sin generar fatiga.

---

## 5. Programa DOU Gang: ¿está bien integrado? ¿genera comportamiento real?

Está **mencionado** en todos lados, pero no está **operacionalizado**:

- Nunca se explicita la tasa de conversión (¿$1 = X coins? ¿100 coins = qué producto exactamente?). Sin esa claridad visible en cada email, el usuario no puede tomar una decisión racional de "me conviene comprar ahora para llegar a tal canje". Un programa de puntos que no muestra el "cuánto me falta" pierde la mayor parte de su poder de conversión.
- No hay barra de progreso ni bloque dinámico de "balance actual de coins" en los emails transaccionales/de campaña (sí se linkea a la página del sitio, pero eso agrega fricción — cada clic perdido es conversión perdida).
- El referido (300 coins + 10% off) es el mecanismo más barato de adquisición que tiene la marca y está reducido a una posdata dentro del email de reseña. Merece flujo propio, con su propio asunto, timing y seguimiento (¿cuántos códigos se comparten? ¿cuántos se canjean?).
- El ranking mensual "Top 5" es divertido para el 0.1% de la base que puede aparecer ahí, pero no genera ningún incentivo para el 99% restante que sabe que nunca va a estar en el Top 5. Un programa de loyalty maduro necesita tiers (ej. Bronze/Silver/Gold, o "Gang Member / Gang Leader / Gang OG") donde el progreso sea alcanzable para cualquiera, no solo competitivo contra los 5 más grandes compradores.
- Los coins no tienen fecha de expiración mencionada — sin urgencia, los coins "juntando polvo" (como dice el propio copy del Flujo 4.2) no generan el efecto de urgencia que deberían.

**Conclusión:** el programa tiene una excelente narrativa de marca pero le falta el andamiaje mecánico (tiers, expiración, visibilidad de balance, canje claro) para efectivamente mover comportamiento de compra más allá del efecto novedad.

---

## 6. Segmentación y personalización: oportunidades perdidas

- Ninguna segmentación por **historial de sabor comprado** (mencionado solo como mejora futura en un flujo).
- Ninguna segmentación por **nivel de engagement** (abre/no abre, clickea/no clickea) — esto es tanto un tema de personalización como de deliverability.
- Ninguna segmentación por **CLV/frecuencia de compra** para dar trato diferencial a los mejores clientes.
- Ninguna segmentación **geográfica** (tiempos y costos de envío distintos AMBA vs. interior podrían cambiar el mensaje de urgencia en Navidad, por ejemplo).
- Ninguna segmentación por **canal de adquisición** (orgánico, paid, referido) para entender qué journey post-compra funciona mejor por origen.
- El A/B testing está bien aplicado a nivel de subject lines, pero no se menciona test de: from-name, horario de envío, longitud de copy, ni de oferta (ej. % off vs. envío gratis vs. coins).
- No hay lógica de "si compró X, no le muestres campaña de descubrimiento de X" (ej. alguien que ya compró el NT no necesita el mismo mensaje de "descubrí el nuevo NT" que alguien que nunca lo probó).

---

## 7. Métricas y KPIs que yo exigiría desde el día 1

**Nivel programa:**
- % de revenue total de ecommerce atribuido a email (benchmark maduro D2C: 25-30%+)
- % de revenue de email que viene de flujos automatizados vs. campañas (los flujos deberían ser 50-70% del revenue de email en un programa sano)
- Tasa de crecimiento neto de lista (altas – bajas – limpiezas)
- Engaged rate de la base (% que abrió o clickeó en los últimos 90 días) — señal de salud de deliverability

**Nivel deliverability (crítico dado el volumen de noviembre):**
- Bounce rate (<2%)
- Spam complaint rate (<0.1%, idealmente <0.03%)
- Tasa de colocación en inbox vs. promociones/spam

**Nivel flujo:**
- Welcome: tasa de conversión a primera compra dentro de los 7 días
- Carrito abandonado: tasa de recuperación (benchmark 5-10% de quienes reciben el flujo)
- Post-compra: tasa de recompra a 30/60/90 días, tasa de reseñas obtenidas, tasa de uso de código de referido
- Recompra (winback temprano): % reactivado sobre la base que entra al flujo
- Cumpleaños: tasa de conversión y AOV comparado al AOV base

**Nivel loyalty:**
- % de coins emitidos que efectivamente se canjean (indicador líder de percepción de valor del programa)
- Revenue incremental atribuible a miembros DOU Gang vs. no miembros
- CAC vía referido vs. CAC pago

**Nivel campaña:**
- Open rate, CTR, CTOR (click-to-open — más honesto que open rate solo)
- Revenue per recipient (RPR) por campaña
- Tasa de baja por campaña (alertar si supera 0.5%)

---

## 8. Riesgos y puntos ciegos

1. **Riesgo de deliverability por sobrecarga de noviembre.** Sin supresión de frecuencia ni segmentación por engagement, el pico de envíos en la temporada de mayor revenue puede terminar dañando la reputación de envío justo quiere que funcione mejor (spam folder en Gmail/Outlook).
2. **Erosión de margen no controlada.** Envío gratis + doble coins + 10% off por referido + posible descuento de Black Friday/CyberMonday pueden apilarse sobre el mismo pedido sin que el documento mencione ningún control de stacking de beneficios. Esto puede volver no rentable una venta que en el papel parece exitosa.
3. **Dependencia de placeholders sin resolver a fecha de ejecución.** "Confirmar con el equipo", "definir oferta concreta", "insertar oferta definida" aparecen en las campañas de mayor revenue del año (CyberMonday, Black Friday). Si esto no se resuelve con antelación, el riesgo de ejecución de último momento es alto.
4. **Cumplimiento de datos personales.** Se recolecta fecha de nacimiento y se usa WhatsApp como canal — no hay ninguna mención de opt-in explícito, política de privacidad ni tratamiento conforme a la Ley 25.326 de Protección de Datos Personales de Argentina. Es un punto ciego legal, no solo de marketing.
5. **Riesgo reputacional en el flujo de reseñas.** Se pide reseña a los 7 días sin ningún mecanismo de derivación previa (ej. "¿todo bien con tu pedido?" antes de pedir reseña pública) — un cliente insatisfecho puede terminar dejando una reseña negativa pública en lugar de ser interceptado primero por soporte.
6. **Falsa urgencia si no hay sincronización real de stock.** El propio documento advierte "usar solo si es real" sobre el dato de que el NT se agota — señal de que el equipo es consciente del riesgo, pero no hay integración automática de inventario a Klaviyo que garantice que ese mensaje sea siempre verídico.
7. **Todo el programa depende de un solo canal (email).** No hay integración con SMS, WhatsApp marketing (no solo soporte) ni retargeting pago basado en segmentos de Klaviyo (ej. audiencias de alto CLV para lookalikes en Meta/Google). Un journey omnicanal recupera más revenue que email aislado.
8. **Ninguna mención de higiene técnica** (SPF/DKIM/DMARC, warm-up de dominio, monitoreo de reputación de IP) — asumible que ya está resuelto, pero no debería darse por sentado en un documento de estrategia.

---

## 9. Qué falta por completo (y que un programa maduro de email sí tiene)

- Programa de **suscripción/recompra automática**.
- Canal y flujo de **regalos corporativos / B2B**.
- **Browse abandonment flow**.
- **Sunset flow** de higiene de lista (re-permission antes de eliminar contactos fríos).
- **Preference center** (frecuencia, sabor favorito, ocasión de compra).
- **Programa VIP con tiers** (más allá del ranking Top 5).
- **Encuesta de zero-party data** en el onboarding (para qué compra, cómo conoció la marca).
- Integración real de **SMS/WhatsApp como canal de marketing**, no solo soporte.
- **Flujo de recuperación de reseñas negativas** (interceptar antes de review pública).
- **Reporting recurrente** (dashboard semanal/mensual de las métricas del punto 7) — sin esto, nadie sabe si el programa está funcionando hasta fin de año.
- **Reglas de supresión y frequency capping** cross-flow/cross-campaña.
- **Testing de deliverability e higiene técnica** documentado como proceso, no solo como copy.

---

## TOP 5 ACCIONES MÁS URGENTES (ordenadas por impacto en revenue)

**1. Lanzar un programa de suscripción/recompra automática.**
Es el mayor lever de CLV disponible y el producto ya tiene la cadencia de consumo ideal (2-3 semanas por caja). Convertir el Flujo 4 de "reconquista manual" a "gestión de suscriptores activos" cambia el negocio de transaccional a recurrente. Impacto: revenue predecible + reducción de CAC efectivo por cliente.

**2. Operacionalizar la economía de DOU Gang: definir la conversión de coins, agregar balance visible/dinámico en cada email relevante, crear tiers alcanzables y darle flujo propio al referido.**
Hoy el loyalty es narrativa sin mecánica. Sin esto, no genera comportamiento sostenido más allá del efecto novedad, y el canal más barato de adquisición (referidos) está subutilizado como posdata.

**3. Reordenar y proteger el calendario de noviembre con reglas de frecuencia y supresión cross-flow/cross-campaña.**
El mes de mayor revenue potencial (lanzamiento + CyberMonday + Black Friday) es también el de mayor riesgo de fatiga y daño a deliverability. Sin supresión de frecuencia, el mismo mes que debería maximizar revenue puede terminar dañando la reputación de envío para todo 2027.

**4. Construir segmentación real: engagement (para deliverability), historial de sabor/CLV (para personalización) y sumar un flujo de browse abandonment.**
Hoy todo el mundo recibe el mismo mensaje sin importar cuánto vale ni qué prefiere. Esto es la diferencia entre un programa de email genérico y uno que realmente compite en revenue-per-subscriber contra los benchmarks del sector.

**5. Formalizar el canal B2B/regalos corporativos y separar el journey de "comprador para uno mismo" del de "comprador para regalar".**
Son dos negocios distintos con distinto AOV y distinta necesidad de mensaje (la ambigüedad detectada en el punto 1 del Flujo de Bienvenida es la punta del iceberg). Corporativo es margen alto y ticket alto sin necesidad de descuentos agresivos — está completamente ausente del plan actual.
