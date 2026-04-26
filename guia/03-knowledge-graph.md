# Slide 3 — Knowledge graph para corpus heterogéneo ⭐

## Tesis

Un knowledge graph es **nodes + edges + triples**. Para corpus heterogéneos (código + docs + papers + imágenes), el grafo es el formato natural: cada doc es un nodo, cada cita/link/import es una arista, cada afirmación contextualizada es un triple. **El grafo emerge si tomas notas con cuidado** — Graphify lo automatiza para corpus donde escribir el grafo a mano no escala.

## Mensajes clave

- **3 primitivas, no más**: Node (entidad nombrada), Edge (relación tipada entre dos nodos), Triple (sujeto-predicado-objeto). Wikipedia, Google Knowledge Graph y este wiki personal son todos lo mismo en el fondo.
- **Por qué bate al RAG vectorial**: vector trocea en chunks y busca por similitud. Si la respuesta requiere *cruzar* dos chunks ("X depende de Y mencionado en cap. 3 del doc A y cap. 7 del doc B"), no la encuentra. El grafo ya hizo el trabajo de *conectar*.
- **La metáfora del bibliotecario** (Karpathy): un chatbot es un oráculo (le preguntas, contesta, olvida). Un bibliotecario **conoce sus libros** — sabe cuáles citan a cuáles, cuáles tratan el mismo tema con posturas distintas, cuáles son fundacionales. Eso es lo que un knowledge graph aporta a un agente.
- **Marcado de confianza**: Graphify obliga a marcar cada arista como `EXTRACTED` (literal en el texto), `INFERRED` (inferida con confianza 0.6-0.9) o `AMBIGUOUS` (incierto, no se omite). Esa transparencia es la diferencia con un RAG ingenuo.

## Cross-refs wiki

- `wiki/conceptos/knowledge-graph.md` — definición canónica con la tabla node/edge/triple y ejemplos.
- `wiki/conceptos/llm-wiki.md` — el patrón Karpathy aplicado.
- `wiki/conceptos/agentic-search-vs-rag.md` — la sección "Graph RAG: la tercera vía".

## Anécdotas posibles

- Wikipedia: cada artículo es un nodo, cada `[[wikilink]]` es un edge. Tras 20 años de edición humana, es uno de los grafos más densos del mundo. Karpathy: "esto es lo que quiero, pero personal y privado".
- Google Knowledge Graph: el panel lateral cuando buscas algo concreto sale del grafo. Cada campo (dirección, horario, arquitecto) es un nodo distinto, no un campo de una row.

## Preguntas tipo

- *"¿No es esto lo mismo que Neo4j / GraphDB?"* → Es la misma estructura, distinta capa. Neo4j es la **base de datos**. Aquí hablamos de la representación lógica que Graphify construye sobre tu corpus, y luego puede *exportar* a Neo4j si quieres (`graphify --neo4j`).
- *"¿Y cómo se mantiene el grafo cuando el corpus cambia?"* → `--update` incremental: Graphify guarda hashes SHA256 de cada fichero y solo re-procesa los modificados. Slide 11 lo desarrolla.

## Transición

"Sabemos qué construimos. Ahora vamos a ver cómo Graphify lo construye en concreto."
