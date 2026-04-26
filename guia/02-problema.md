# Slide 2 — El problema

## Tesis

El context window es finito y el corpus crece. Si tu agente vuelve a leer 40 ficheros cada sesión para responder *"qué depende de la función X"*, has reventado el [instruction budget](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/conceptos/instruction-budget.md) y los outputs se vuelven aleatorios.

## Mensajes clave

- **El número clave**: ~150-200 instrucciones por context window que un modelo frontier puede seguir con consistencia. Pasar de ahí = el agente atiende a medias y se salta pasos.
- **Cómo se gasta el presupuesto** sin enterarte: prompt explícito (20-100) + CLAUDE.md/AGENTS.md (50-200) + descripciones de tools (5-15 cada una × 10 tools internas + 3 MCP servers) + skills (20-100 cada una). Suma rápida: 250-400. Estás fuera.
- **Releer ficheros raw** cada vez para responder lo mismo es el síntoma más común del problema. Se siente como "el modelo es tonto hoy" pero en realidad es "no le queda budget para atender mi prompt".
- **La salida**: un grafo persistente que cabe en 3-5k tokens y responde la misma pregunta sin re-lectura. Eso es Graphify (o GitNexus para código puro, ver slide 6).

## Cross-refs wiki

- `wiki/conceptos/instruction-budget.md` — el concepto central.
- `wiki/conceptos/coste-total-tokens.md` — coste real de un workflow, no solo el prompt.
- `wiki/conceptos/agentic-search-vs-rag.md` — agentic search puro como alternativa válida cuando el corpus cabe.

## Anécdotas posibles

- Dexter Horthy (HumanLayer) cuenta que el prompt monolítico de planning RPI v1 tenía 85 instrucciones, y el modelo se saltaba pasos críticos para la mitad de los usuarios. Solución: descomponer en 7 sub-fases (CRISPI). No un grafo, pero misma raíz: respeta el budget.
- En el experimento real de Graphify sobre el wiki de Código Sin Siesta: 22-30× reducción de tokens por consulta. No los 71× del marketing. Pero igualmente: pasar de ~76k tokens a ~3.5k por consulta.

## Preguntas tipo

- *"¿Por qué no aumentamos el context window?"* → Existen modelos de 1M, 2M tokens. Pero el **instruction budget** es ortogonal al window: 200 instrucciones es un límite cognitivo del modelo, no del tamaño físico del prompt. Más espacio no compra más atención.
- *"¿RAG vectorial no resuelve esto?"* → A medias. Vector mete chunks aleatorios; si la respuesta vive *entre dos documentos*, no la encuentra. El grafo formaliza la conexión. Slide 3 desarrolla.
- *"¿No es prematuro? Mi repo tiene 10 ficheros."* → Sí, lo es. Slide 7 lo dice explícito: si el corpus cabe en context, no necesitas Graphify.

## Transición

"Si el context budget es finito y los corpus crecen, la salida es algo que dure entre sesiones. Eso es un knowledge graph."
