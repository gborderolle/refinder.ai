<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome file</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__html"><h1 id="refinder.ai-–-plan-completo-del-proyecto-mvp-1--producto-final-–-versión-ampliada-casos-de-uso--multimodal"><a href="http://refinder.ai">refinder.ai</a> – plan completo del proyecto (mvp 1 + producto final) – versión ampliada (casos de uso + multimodal)</h1>
<p>fecha: 2025-12-10<br>
versión: 1.1</p>
<h2 id="resumen-ejecutivo">1. resumen ejecutivo</h2>
<p><strong><a href="http://refinder.ai">refinder.ai</a></strong> es una solución de <em>conversational commerce</em> que convierte <strong>whatsapp</strong> en un canal de ventas y atención para comercios con catálogo (arranque: ferreterías).<br>
El usuario escribe o habla por whatsapp (“¿tenés taladro percutor?”, “necesito tornillos m8”), o envía <strong>fotos</strong> (pieza, herramienta, tornillo) y el asistente con IA responde con <strong>productos</strong>, <strong>precio</strong>, <strong>stock por sucursal</strong>, alternativas y, en etapas posteriores, permite <strong>armar el pedido</strong>, <strong>pagar</strong> y <strong>coordinar entrega</strong>.</p>
<p>El plan se divide en:</p>
<ul>
<li><strong>mvp 1 (monocliente)</strong>: producto funcional para una <strong>única cadena</strong> (una marca) con <strong>3 locales</strong>.</li>
<li><strong>producto final (saas multitenant)</strong>: plataforma para múltiples clientes con aislamiento por tenant, personalización y administración completa.</li>
</ul>
<h2 id="objetivos-de-negocio">2. objetivos de negocio</h2>
<ul>
<li>reducir el tiempo de respuesta a consultas por whatsapp (atención 24/7).</li>
<li>disminuir carga operativa del equipo de ventas (menos “búsquedas manuales”).</li>
<li>aumentar conversión desde whatsapp (catálogo + recomendación + pedido).</li>
<li>habilitar escalabilidad: del cliente piloto a muchos comercios con mínima fricción.</li>
</ul>
<h2 id="alcance-por-etapa">3. alcance por etapa</h2>
<h3 id="mvp-1-monocliente">3.1 mvp 1 (monocliente)</h3>
<p><strong>foco</strong>: resolver búsqueda de producto + disponibilidad por sucursal con experiencia conversacional robusta e incluir multimodal “controlado”.</p>
<p>incluye:</p>
<ul>
<li>ingestión de catálogo y stock (inicialmente por excel/csv).</li>
<li>normalización de productos (sku, marca, atributos, medidas).</li>
<li>búsqueda semántica + búsqueda exacta por códigos.</li>
<li>bot de whatsapp con IA y <em>guardrails</em> (respuestas consistentes, no inventar stock/precio).</li>
<li>panel web para administrar catálogo/stock y ver conversaciones.</li>
<li><strong>multimodal básico</strong>:
<ul>
<li>foto: identificación aproximada / sugerencias (“parece una llave ajustable…”).</li>
<li>audio: transcripción a texto y tratamiento como consulta normal.</li>
</ul>
</li>
<li>logging y observabilidad mínimos (auditoría de conversaciones, errores).</li>
</ul>
<p>fuera de alcance (mvp 1):</p>
<ul>
<li>multitenancy.</li>
<li>checkout y pagos.</li>
<li>integración real-time con erp.</li>
<li>logística automatizada (puede simularse con “pedido creado”).</li>
<li>visión “industrial” (detección precisa de piezas específicas sin dataset).</li>
</ul>
<h3 id="producto-final-saas-multitenant">3.2 producto final (saas multitenant)</h3>
<p><strong>foco</strong>: convertirlo en un producto vendible a múltiples comercios y subir el nivel multimodal.</p>
<p>incluye:</p>
<ul>
<li>multitenancy completo (aislamiento de datos, configuración por tenant).</li>
<li>onboarding de tenants (alta rápida, carga de catálogo, configuración de whatsapp).</li>
<li>roles y permisos por tenant.</li>
<li>integraciones por tenant: pagos, logística, erp (conectores).</li>
<li>métricas comerciales y operativas (conversión, top consultas, tasa de no-match).</li>
<li>escalado horizontal de bot/ia (colas + workers).</li>
<li>hardening de seguridad y compliance.</li>
<li><strong>multimodal avanzado</strong>:
<ul>
<li>fotos: clasificación + extracción de señales (texto, marca, modelo, medidas aproximadas), y sugerencia de productos compatibles.</li>
<li>audios: soporte de notas de voz y dictado; detección de intención + entidades.</li>
</ul>
</li>
</ul>
<h2 id="tecnologías-alineadas-con-flowence">4. tecnologías (alineadas con Flowence)</h2>
<p>Se reutiliza el stack que ya domináis en Flowence para minimizar riesgo y acelerar entrega.</p>
<h3 id="backend-principal-core-saas">4.1 backend principal (core saas)</h3>
<ul>
<li><strong>.NET 8</strong> + <strong>ABP Framework</strong> (multi-tenancy, auth, permisos, auditoría, background jobs).</li>
<li><strong>MySQL</strong> (persistencia principal).</li>
<li>enfoque modular: Catálogo, Stock, Conversaciones, Pedidos, Integraciones, Medios (multimodal).</li>
</ul>
<h3 id="frontend">4.2 frontend</h3>
<ul>
<li><strong>Angular</strong> (alineado con vuestra base).</li>
<li>panel admin para:
<ul>
<li>productos y categorías</li>
<li>sucursales</li>
<li>stock y precios</li>
<li>importaciones</li>
<li>logs de conversaciones / feedback</li>
<li>revisión de “no match” y corrección de catálogo</li>
</ul>
</li>
</ul>
<h3 id="servicio-de-ia-microservicio">4.3 servicio de IA (microservicio)</h3>
<ul>
<li><strong>Python 3.12</strong></li>
<li>SDK de OpenAI &gt;=1.0.0 (interfaz moderna asincrónica).</li>
<li>responsabilidades:
<ul>
<li>parsing de intención (buscar producto, consultar stock, precios, compatibilidades)</li>
<li>retrieval (búsqueda) y construcción de respuestas</li>
<li><em>guardrails</em>: no alucinar stock/precios; citar “fuente” interna</li>
<li>prompts versionados + pruebas</li>
</ul>
</li>
</ul>
<h3 id="multimodal-visión--audio">4.4 multimodal (visión + audio)</h3>
<ul>
<li><strong>visión</strong>:
<ul>
<li>análisis de imagen con modelo multimodal (p.ej. “vision”).</li>
<li>OCR cuando aplique (etiquetas, códigos, números de parte).</li>
</ul>
</li>
<li><strong>audio</strong>:
<ul>
<li>STT (speech-to-text) para notas de voz.</li>
<li>normalización del transcript (puntuación, unidades, medidas).</li>
</ul>
</li>
</ul>
<h3 id="mensajería--integración-whatsapp">4.5 mensajería / integración whatsapp</h3>
<ul>
<li>integración vía <strong>Meta WhatsApp Business Cloud API</strong> o proveedor.</li>
<li>webhooks hacia backend / gateway.</li>
<li><strong>colas</strong> (recomendado en producto final) para desacoplar ingestión de mensajes y procesamiento IA.</li>
</ul>
<h3 id="infraestructura">4.6 infraestructura</h3>
<ul>
<li>entornos separados (dev/qa/staging/prod), idealmente un VPS por entorno como ya hacéis.</li>
<li>reverse proxy (Apache/Nginx) y certificados SSL.</li>
<li>observabilidad: logs estructurados + métricas + tracing (OpenTelemetry recomendado).</li>
<li>almacenamiento de medios: S3 compatible o storage local con políticas de retención.</li>
</ul>
<h2 id="arquitectura-propuesta">5. arquitectura propuesta</h2>
<h3 id="componentes">5.1 componentes</h3>
<ul>
<li><strong>whatsapp gateway</strong> (webhook + envío de mensajes):
<ul>
<li>recibe mensajes entrantes (texto, imagen, audio, documentos)</li>
<li>normaliza payload</li>
<li>aplica rate limiting, deduplicación e idempotencia</li>
</ul>
</li>
<li><strong>media ingestion service</strong> (puede vivir dentro del core api al principio):
<ul>
<li>descarga medios desde el provider</li>
<li>los almacena (con cifrado/retención)</li>
<li>genera metadatos (tipo, tamaño, hash)</li>
</ul>
</li>
<li><strong>core api (abp/.net)</strong>:
<ul>
<li>auth, tenants, catálogos, stock, precios, sucursales</li>
<li>conversación: estado de sesión, eventos, auditoría</li>
<li>pedidos (fases posteriores)</li>
</ul>
</li>
<li><strong>ai service (python)</strong>:
<ul>
<li>interpreta intención (texto/audio transcrito)</li>
<li>interpreta imagen (visión + OCR)</li>
<li>consulta catálogo/stock vía api interna</li>
<li>genera respuesta</li>
</ul>
</li>
<li><strong>db MySQL</strong></li>
<li><strong>admin web (angular)</strong></li>
</ul>
<h3 id="secuencia-básica-mvp-1-–-texto--audio--imagen">5.2 secuencia básica (mvp 1) – texto / audio / imagen</h3>
<ol>
<li>cliente envía mensaje a whatsapp (texto, audio o imagen)</li>
<li>whatsapp → webhook → gateway</li>
<li>gateway registra evento y, si hay medios, solicita a media ingestion descargar/guardar</li>
<li>gateway llama a <strong>ai service</strong> con:
<ul>
<li>texto (si aplica)</li>
<li>transcript (si audio)</li>
<li>url interna/metadata (si imagen)</li>
</ul>
</li>
<li>ai service llama a <strong>core api</strong> para buscar productos/stock</li>
<li>ai service devuelve respuesta estructurada</li>
<li>gateway envía respuesta a whatsapp</li>
<li>core api guarda auditoría y métricas</li>
</ol>
<h3 id="diagrama-mermaid">5.3 diagrama (mermaid)</h3>
<pre class=" language-mermaid"><code class="prism  language-mermaid">flowchart LR
  U[cliente whatsapp] --&gt; W[whatsapp provider]
  W --&gt; G[whatsapp gateway]
  G --&gt; M[media ingestion]
  G --&gt; A[ai service (python)]
  A --&gt; C[core api (.net 8 + abp)]
  C --&gt; D[(mysql)]
  C --&gt; P[admin web (angular)]
  M --&gt; S[(media storage)]
  G --&gt; W
</code></pre>
<h2 id="modelo-de-datos-mvp-1-–-ampliado-para-multimodal">6. modelo de datos (mvp 1) – ampliado para multimodal</h2>
<p>tablas principales (simplificado):</p>
<ul>
<li><code>branches</code>:
<ul>
<li>id, name, address, geo_lat, geo_lng, opening_hours</li>
</ul>
</li>
<li><code>products</code>:
<ul>
<li>id, sku, internal_code, name, brand, description, category_id, attributes_json, image_url</li>
</ul>
</li>
<li><code>price_lists</code>:
<ul>
<li>id, name, currency</li>
</ul>
</li>
<li><code>product_prices</code>:
<ul>
<li>product_id, price_list_id, amount</li>
</ul>
</li>
<li><code>stocks</code>:
<ul>
<li>branch_id, product_id, quantity, updated_at</li>
</ul>
</li>
<li><code>whatsapp_contacts</code>:
<ul>
<li>id, phone_e164, display_name</li>
</ul>
</li>
<li><code>conversations</code>:
<ul>
<li>id, whatsapp_contact_id, status, started_at, last_message_at</li>
</ul>
</li>
<li><code>conversation_messages</code>:
<ul>
<li>id, conversation_id, direction(in/out), message_type(text/image/audio), text, provider_message_id, created_at</li>
</ul>
</li>
<li><code>message_attachments</code>:
<ul>
<li>id, conversation_message_id, media_type(image/audio/document), provider_media_id, storage_url, sha256, size_bytes, mime_type, created_at</li>
</ul>
</li>
<li><code>audio_transcripts</code>:
<ul>
<li>id, attachment_id, transcript_text, language, confidence, duration_ms, created_at</li>
</ul>
</li>
<li><code>image_analyses</code>:
<ul>
<li>id, attachment_id, summary, ocr_text, detected_labels_json, created_at</li>
</ul>
</li>
<li><code>ai_requests</code>:
<ul>
<li>id, conversation_id, prompt_version, model, tokens_in, tokens_out, latency_ms, created_at, trace_id</li>
</ul>
</li>
</ul>
<h2 id="búsqueda-y-“rags”-retrieval--grounding">7. búsqueda y “rags” (retrieval + grounding)</h2>
<h3 id="estrategia-de-búsqueda-mvp-1">7.1 estrategia de búsqueda (mvp 1)</h3>
<ul>
<li><strong>búsqueda exacta</strong>: por sku/código interno.</li>
<li><strong>búsqueda semántica</strong>: embeddings sobre <code>name</code>, <code>brand</code>, <code>description</code>, atributos y sinónimos.</li>
<li><strong>multimodal</strong>:
<ul>
<li>imagen: generar una descripción corta + OCR → convertir a consulta enriquecida.</li>
<li>audio: transcript → consulta textual normal.</li>
</ul>
</li>
</ul>
<h3 id="guardrails-críticos">7.2 guardrails críticos</h3>
<ul>
<li>nunca inventar stock/precio:
<ul>
<li>respuesta debe basarse en datos de <code>stocks</code> y <code>product_prices</code>.</li>
</ul>
</li>
<li>si no hay match:
<ul>
<li>pedir aclaración (medida, marca, uso, foto adicional).</li>
</ul>
</li>
<li>si hay varios matches:
<ul>
<li>devolver top N con opciones y pedir confirmación.</li>
</ul>
</li>
<li>si la imagen no es concluyente:
<ul>
<li>responder con probabilidad (“parece…”, “podría ser…”) y pedir validación.</li>
</ul>
</li>
</ul>
<h3 id="normalización-de-catálogo-recomendado">7.3 normalización de catálogo (recomendado)</h3>
<ul>
<li>tabla de sinónimos (ej. “amoladora” = “esmeriladora”).</li>
<li>parser de medidas (mm, pulgadas) y unidades.</li>
<li>reglas por rubro ferretería: tornillería, herramientas, electricidad, sanitaria.</li>
</ul>
<h2 id="catálogo-de-casos-de-uso-aplicación">8. catálogo de casos de uso (aplicación)</h2>
<h3 id="actores">8.1 actores</h3>
<ul>
<li><strong>cliente final</strong>: persona que escribe/manda audio/foto por whatsapp.</li>
<li><strong>operador ferretería</strong> (mvp 1): recibe derivaciones, revisa pedidos o consultas no resueltas.</li>
<li><strong>admin ferretería</strong>: gestiona catálogo, precios, stock, reglas y sucursales.</li>
<li><strong>admin plataforma</strong> (producto final): gestiona tenants, planes, límites y soporte.</li>
</ul>
<h3 id="casos-de-uso-principales-mvp-1">8.2 casos de uso principales (mvp 1)</h3>
<h4 id="cu-01-consultar-producto-por-texto">cu-01: consultar producto por texto</h4>
<ul>
<li>actor: cliente final</li>
<li>precondición: contacto válido; catálogo cargado</li>
<li>flujo:
<ol>
<li>usuario envía texto (“tornillos m8 para madera”)</li>
<li>sistema interpreta intención + entidades (tipo, medida, uso)</li>
<li>busca productos y devuelve top 3</li>
<li>muestra stock por sucursal + precio</li>
<li>pide confirmación si hay ambigüedad</li>
</ol>
</li>
<li>alternativos:
<ul>
<li>no match → solicita aclaración (medida, marca) y sugiere categorías</li>
</ul>
</li>
</ul>
<h4 id="cu-02-consultar-producto-por-foto-identificación">cu-02: consultar producto por foto (identificación)</h4>
<ul>
<li>actor: cliente final</li>
<li>objetivo: identificar qué producto es o cuál comprar</li>
<li>flujo:
<ol>
<li>usuario envía foto de una pieza/herramienta</li>
<li>sistema descarga y analiza imagen (visión + OCR)</li>
<li>genera una descripción (“parece una válvula esférica de 1/2””)</li>
<li>busca productos compatibles</li>
<li>responde con opciones + preguntas de confirmación (“¿medida 1/2 o 3/4?”)</li>
</ol>
</li>
<li>alternativos:
<ul>
<li>imagen borrosa → pedir nueva foto con recomendaciones (buena luz, acercar etiqueta)</li>
<li>detecta texto/modelo → prioriza búsqueda exacta por código</li>
</ul>
</li>
</ul>
<h4 id="cu-03-consultar-por-audio-nota-de-voz">cu-03: consultar por audio (nota de voz)</h4>
<ul>
<li>actor: cliente final</li>
<li>flujo:
<ol>
<li>usuario envía nota de voz (“preciso una amoladora grande, de 9 pulgadas”)</li>
<li>STT → transcript</li>
<li>NLU sobre transcript</li>
<li>búsqueda + respuesta como cu-01</li>
</ol>
</li>
<li>alternativos:
<ul>
<li>transcript con baja confianza → pedir confirmación (“entendí ‘9 pulgadas’, ¿es correcto?”)</li>
</ul>
</li>
</ul>
<h4 id="cu-04-consultar-stock-por-sucursal">cu-04: consultar stock por sucursal</h4>
<ul>
<li>actor: cliente final</li>
<li>flujo:
<ol>
<li>usuario pregunta “¿hay stock en el local del centro?”</li>
<li>sistema identifica producto (por contexto conversacional o por selección previa)</li>
<li>responde stock por esa sucursal y alternativas en otras</li>
</ol>
</li>
</ul>
<h4 id="cu-05-pedir-recomendación-“qué-me-conviene-usar”">cu-05: pedir recomendación (“qué me conviene usar”)</h4>
<ul>
<li>actor: cliente final</li>
<li>ejemplo: “tengo que fijar un estante a pared de ladrillo, ¿qué tarugo uso?”</li>
<li>flujo:
<ol>
<li>sistema detecta el problema/tarea (uso)</li>
<li>pregunta datos mínimos (peso, tipo de pared, diámetro)</li>
<li>sugiere 2–3 opciones (tarugo + tornillo) y explica brevemente</li>
<li>muestra stock por sucursal</li>
</ol>
</li>
<li>guardrail:
<ul>
<li>evitar recomendaciones riesgosas; si hay riesgo, sugerir asesoramiento humano</li>
</ul>
</li>
</ul>
<h4 id="cu-06-derivación-a-humano">cu-06: derivación a humano</h4>
<ul>
<li>actor: cliente final / operador</li>
<li>flujo:
<ol>
<li>usuario pide hablar con alguien o el bot detecta baja confianza</li>
<li>sistema crea “ticket” y notifica a operador (panel o canal interno)</li>
<li>operador responde y el bot queda en modo “pasivo” para esa conversación</li>
</ol>
</li>
</ul>
<h4 id="cu-07-administración-de-catálogo">cu-07: administración de catálogo</h4>
<ul>
<li>actor: admin ferretería</li>
<li>flujo:
<ol>
<li>carga masiva (excel/csv)</li>
<li>validaciones + previsualización</li>
<li>confirmación de import</li>
<li>índices de búsqueda se regeneran (embeddings)</li>
</ol>
</li>
<li>alternativos:
<ul>
<li>filas inválidas → reporte de errores descargable</li>
</ul>
</li>
</ul>
<h4 id="cu-08-administración-de-stock-por-sucursal">cu-08: administración de stock por sucursal</h4>
<ul>
<li>actor: admin ferretería</li>
<li>flujo:
<ol>
<li>actualizar stock manual o por import incremental</li>
<li>registrar quién cambió qué (auditoría)</li>
<li>reflejar disponibilidad en respuestas del bot</li>
</ol>
</li>
</ul>
<h3 id="casos-de-uso-producto-final-multitenant">8.3 casos de uso (producto final multitenant)</h3>
<h4 id="cu-09-onboarding-de-tenant">cu-09: onboarding de tenant</h4>
<ul>
<li>actor: admin plataforma / admin tenant</li>
<li>flujo:
<ol>
<li>crear tenant</li>
<li>configurar marca, moneda, horarios, reglas</li>
<li>conectar whatsapp (token/webhook)</li>
<li>cargar catálogo y stock</li>
<li>pruebas de mensajes</li>
<li>activar producción</li>
</ol>
</li>
</ul>
<h4 id="cu-10-personalización-del-bot-por-tenant">cu-10: personalización del bot por tenant</h4>
<ul>
<li>actor: admin tenant</li>
<li>opciones:
<ul>
<li>tono (formal/informal)</li>
<li>plantillas de respuesta</li>
<li>mensajes legales</li>
<li>“fallbacks” y derivación a humano</li>
<li>límites y rate limiting</li>
</ul>
</li>
</ul>
<h4 id="cu-11-pedido-conversacional-mvp-2--producto-final">cu-11: pedido conversacional (mvp 2 / producto final)</h4>
<ul>
<li>actor: cliente final</li>
<li>flujo:
<ol>
<li>usuario elige un producto (de una lista)</li>
<li>indica cantidad</li>
<li>confirma sucursal o entrega</li>
<li>sistema genera pedido con estado “pendiente”</li>
<li>notificación al operador / ERP</li>
</ol>
</li>
</ul>
<h4 id="cu-12-pago-y-confirmación-producto-final">cu-12: pago y confirmación (producto final)</h4>
<ul>
<li>actor: cliente final</li>
<li>flujo:
<ol>
<li>sistema genera checkout/link de pago</li>
<li>usuario paga</li>
<li>webhook de pasarela confirma</li>
<li>pedido pasa a “pagado”</li>
<li>se crea envío (si aplica)</li>
</ol>
</li>
</ul>
<h4 id="cu-13-entrega-a-domicilio-y-tracking">cu-13: entrega a domicilio y tracking</h4>
<ul>
<li>actor: cliente final</li>
<li>flujo:
<ol>
<li>se cotiza envío</li>
<li>se genera shipment con operador</li>
<li>se envía tracking por whatsapp</li>
<li>eventos de entrega actualizan estados</li>
</ol>
</li>
</ul>
<h2 id="ejemplos-de-conversaciones-multimodales-whatsapp">9. ejemplos de conversaciones multimodales (whatsapp)</h2>
<h3 id="foto-→-identificación-y-oferta">9.1 foto → identificación y oferta</h3>
<p><strong>usuario</strong>: (envía foto de una pieza metálica con rosca) “¿qué es esto? necesito comprarla”<br>
<strong>bot</strong>: “por la foto parece un <strong>niple roscado</strong>. ¿me confirmás el diámetro? (1/2, 3/4 o 1 pulgada). Si podés, mandá una foto del lado donde se ve la medida o la caja.”<br>
<strong>usuario</strong>: “3/4”<br>
<strong>bot</strong>: “ok. tengo estas opciones:</p>
<ol>
<li>niple galvanizado 3/4 x 5 cm – $… – stock: centro 4, cordón 2, paso 0</li>
<li>niple galvanizado 3/4 x 10 cm – $… – stock: centro 6, cordón 1, paso 3<br>
¿cuál querés?”</li>
</ol>
<h3 id="audio-→-transcripción-y-búsqueda">9.2 audio → transcripción y búsqueda</h3>
<p><strong>usuario</strong>: (audio) “hola, preciso un taladro percutor bosch, y me decís si hay en el centro”<br>
<strong>bot</strong>: “perfecto. encontré 2 taladros percutores Bosch. stock en <strong>centro</strong>:</p>
<ul>
<li>bosch gsb 13 re – 2 unidades – $…</li>
<li>bosch gsb 16 re – 0 unidades – $…<br>
¿querés el gsb 13 re?”</li>
</ul>
<h3 id="foto--recomendación-uso">9.3 foto + recomendación (uso)</h3>
<p><strong>usuario</strong>: (envía foto de una pared con grieta) “¿qué me recomendás para tapar esto?”<br>
<strong>bot</strong>: “parece una grieta fina en pared interior. para recomendarte bien: ¿es interior/exterior y la pared está húmeda?<br>
si es interior y seco: masilla acrílica + espátula. si es exterior o hay humedad: sellador/elastomérico.<br>
¿me confirmás interior o exterior?”</p>
<h2 id="requisitos-no-funcionales-rnfs-–-ampliados-por-multimodal">10. requisitos no funcionales (rnfs) – ampliados por multimodal</h2>
<ul>
<li><strong>privacidad y medios</strong>:
<ul>
<li>almacenar imágenes/audios con retención configurable (por defecto, limitada).</li>
<li>hash de archivo para deduplicación.</li>
<li>evitar guardar más PII de la necesaria.</li>
</ul>
</li>
<li><strong>seguridad</strong>:
<ul>
<li>URLs firmadas para acceso a medios; no exponer enlaces públicos.</li>
<li>validación de firmas del provider en webhooks.</li>
</ul>
</li>
<li><strong>costes</strong>:
<ul>
<li>controlar uso de visión/STT (son más caros que texto).</li>
<li>política: intentar resolver por texto; usar visión/STT sólo si el usuario envía medios.</li>
</ul>
</li>
<li><strong>latencia</strong>:
<ul>
<li>si análisis de imagen tarda, responder primero “lo estoy analizando…” y luego el resultado (si el provider lo permite) o esperar respuesta única.</li>
</ul>
</li>
</ul>
<h2 id="plan-de-entrega-roadmap-–-actualizado">11. plan de entrega (roadmap) – actualizado</h2>
<h3 id="fase-0-–-diseño-1–2-semanas">fase 0 – diseño (1–2 semanas)</h3>
<ul>
<li>definición de dominio (catálogo/stock/conversación/medios)</li>
<li>esquema de BD inicial + tablas de attachments</li>
<li>definición de intents y <em>guardrails</em> (incluye multimodal)</li>
<li>decisión de proveedor whatsapp</li>
<li>prototipo:
<ul>
<li>embeddings sobre catálogo</li>
<li>STT para audio</li>
<li>“vision prompt” para imágenes</li>
</ul>
</li>
</ul>
<h3 id="fase-1-–-mvp-1-monocliente-4–6-semanas">fase 1 – mvp 1 monocliente (4–6 semanas)</h3>
<ul>
<li>core api (productos, sucursales, stock, import, mensajes, attachments)</li>
<li>ai service python (texto + audio + imagen)</li>
<li>whatsapp gateway (webhooks + envío)</li>
<li>panel admin angular (mínimo)</li>
<li>logging + auditoría</li>
<li>piloto en producción</li>
</ul>
<h3 id="fase-2-–-hardening--conversión-2–4-semanas">fase 2 – hardening + conversión (2–4 semanas)</h3>
<ul>
<li>normalización (sinónimos, medidas)</li>
<li>dashboards de “no encontrados”</li>
<li>derivación a humano</li>
<li>tuning de prompts y políticas multimodales</li>
</ul>
<h3 id="fase-3-–-producto-final-multitenant-8–12-semanas">fase 3 – producto final multitenant (8–12 semanas)</h3>
<ul>
<li>ABP multi-tenancy completo</li>
<li>onboarding de tenants</li>
<li>configuración por tenant</li>
<li>aislamiento de datos + pruebas</li>
<li>colas/workers</li>
<li>límites por plan + billing básico</li>
</ul>
<h3 id="fase-4-–-integraciones-variable">fase 4 – integraciones (variable)</h3>
<ul>
<li>pagos por tenant</li>
<li>delivery por tenant</li>
<li>conectores ERP</li>
</ul>
<h2 id="definición-de-“done”-por-etapa-multimodal">12. definición de “done” por etapa (multimodal)</h2>
<h3 id="mvp-1-está-listo-cuando">mvp 1 está listo cuando</h3>
<ul>
<li>texto: top 1–3 correcto con stock/precio correctos.</li>
<li>audio: transcripción estable para voz clara y tratamiento como texto.</li>
<li>imagen: al menos:
<ul>
<li>propone categoría correcta en la mayoría de casos (“herramienta”, “tornillo”, “válvula”)</li>
<li>pide aclaraciones adecuadas cuando no puede concluir</li>
</ul>
</li>
<li>auditoría completa: mensajes + adjuntos + decisiones.</li>
</ul>
<h3 id="producto-final-está-listo-cuando">producto final está listo cuando</h3>
<ul>
<li>onboarding de tenant &lt; 1 hora (asistido o self-service).</li>
<li>aislamiento completo.</li>
<li>control de costes por tenant (límites de visión/STT).</li>
<li>métricas por tenant y globales.</li>
</ul>
<h2 id="riesgos-y-mitigaciones-multimodal">13. riesgos y mitigaciones (multimodal)</h2>
<ul>
<li><strong>imágenes difíciles</strong> (piezas parecidas) → pedir confirmaciones, OCR de packaging, preguntas guiadas.</li>
<li><strong>audio ruidoso</strong> → reintentos y confirmación de entidades detectadas.</li>
<li><strong>contenido sensible</strong> → filtros y respuestas seguras; retención mínima.</li>
<li><strong>alucinación por visión</strong> → respuestas probabilísticas + validación contra catálogo.</li>
</ul>
<hr>
<p>fin del documento.</p>
</div>
</body>

</html>
