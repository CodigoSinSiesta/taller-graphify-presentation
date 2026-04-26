# Slide 9 — Hands-on 2: Skill + queries ⭐

## Tesis

Un grafo que solo se mira en HTML es decorativo. El valor real es que tu agente lo consulte automáticamente cuando trabajas. En 20-25 min cada asistente prueba **3 queries reales** vía skill y compara directamente contra `rg`+lectura para sentir cuándo gana cada cosa.

## Estructura del slide

Cuatro pasos numerados:

1. **Instala la skill** en tu agente (3 min) — `graphify claude install` (o equivalente para tu agente).
2. **Tres queries dirigidas** (10 min) — `/graphify query "..."`, `/graphify path A B`, `/graphify explain X`. Cada una con criterio de éxito explícito.
3. **Contraste con `rg` puro** (5 min) — misma pregunta, dos vías, anota tiempo / tokens / calidad / ruido.
4. **Save-result loop** (3 min) — guarda una buena respuesta de vuelta al grafo con `graphify save-result`. Cierre del loop de aprendizaje.

## Las tres queries del paso 2

| Modo | Comando | Pregunta sobre el starter | Lo que debería responder bien |
|------|---------|---------------------------|-------------------------------|
| BFS | `/graphify query "..."` | "qué conecta el tool registry con la decisión del LLM de invocar grep" | El camino chat.ts → tool-registry → tool-grep + cita cross-modal a `01-anthropic-skills.md` |
| Path | `/graphify path "auth" "chat"` | shortest path entre dos conceptos | 2-3 hops vía server.ts |
| Explain | `/graphify explain "tool-registry"` | resumen + vecinos + dónde se cita | nodo + conexiones + cita en docs |

## Mensajes clave para el ponente

- **El paso 3 es el más pedagógico**. El asistente abre dos terminales: una para `rg` puro, otra para la skill. **Misma pregunta, dos vías.** Y anota tiempo, tokens consumidos por su agente, calidad subjetiva y ruido. Esa tabla es lo que cada asistente se lleva — no Graphify funcionando, sino *opinión razonada* sobre cuándo el grafo gana.
- **Insistir en que `rg` no es enemigo**. Es la baseline contra la que Graphify se mide. En muchos casos (codebase pequeño, pregunta puntual) `rg` gana. Eso no descalifica Graphify — confirma cuándo *no* lo necesitas.
- **El save-result loop** es lo que diferencia un grafo de un RAG sin estado. Cada Q&A cerrada bien se vuelve nodo nuevo. Patrón [agente-aprendiz](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/conceptos/agente-aprendiz.md): la memoria operativa se materializa en el grafo.

## Cross-refs wiki

- `ejercicios/02-skill-queries.md` del starter — el guion paso a paso.
- `wiki/conceptos/agente-aprendiz.md` — el patrón save-result.
- `wiki/conceptos/agentic-search-vs-rag.md` — la baseline que defendemos.

## Anécdotas posibles

- "En mis pruebas sobre el starter, la query `path "auth" "chat"` me devolvió 'no path'. Inicialmente pensé que era bug. Mirando el grafo, el camino auth → server → chat es un camino *vía un middleware*, y el extractor no marcó la arista middleware con tipo apropiado. Lección: el grafo es **literal** sobre lo que el extractor capturó, no semántico. Saber esto te hace un usuario más crítico."
- "La pregunta 'qué conecta tool-registry con grep' la responde el grafo en 1 query (~280 tokens consumidos por mi agente para la respuesta) vs 3 lecturas con `rg`+`cat` (~3.500 tokens). 12× ahorro en una pregunta concreta. Multiplica por 30 preguntas/día en un repo activo y la economía empieza a tener sentido."

## Preguntas tipo

- *"`/graphify query` me devuelve 'no matching nodes'"* → Probablemente ningún nodo del grafo tiene un label que matchea las palabras de tu pregunta. Reformula con términos que aparezcan literal en god nodes (los del `GRAPH_REPORT.md`).
- *"¿La skill se ejecuta cada vez que pregunto algo al agente?"* → No. Hay un PreToolUse hook que **sugiere** consultar el grafo cuando la pregunta encaja con su shape, pero el agente decide. Lo puedes invocar explícitamente con `/graphify query`.

## Transición

"Lo has consultado tú. La siguiente capa: que **otros agentes** consulten tu grafo sin tener que tener Graphify instalado. Eso es MCP."
