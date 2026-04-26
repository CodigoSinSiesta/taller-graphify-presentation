# Slide 5 — Outputs y para qué

## Tesis

Graphify produce 5 outputs y cada uno tiene un consumidor distinto. La trampa frecuente es generar todo y solo mirar el HTML. **El output que más rentabiliza el coste del primer pase es `graph.json` consumido vía skill, no el HTML decorativo.**

## Mensajes clave

| Output | Consumidor | Cuándo abrir |
|--------|-----------|--------------|
| `graph.json` | Tu agente vía skill (`/graphify query`, `/graphify path`) | Siempre — es el activo persistente |
| `GRAPH_REPORT.md` | Tú, lectura humana | Justo después del primer pase |
| `graph.html` | Tú, exploración visual | Cuando el reporte te llama la atención sobre algo concreto |
| `obsidian/` (opt-in `--obsidian`) | Tu vault si tienes uno | Si Obsidian es ya tu workflow |
| MCP server (`--mcp`) | Otros agentes (Claude Desktop, Cursor, etc.) | Cuando varios agentes deben compartir el grafo |

## Detalle por output

- **`graph.json`** (50-500 KB): formato NetworkX serializado. **El activo real**. Cabe en repo o en `.gitignore` según política. Versionable. Re-consultable indefinidamente.
- **`GRAPH_REPORT.md`** (10-30 KB): legible. Siempre tiene 3 secciones críticas: God Nodes (top conectados), Surprising Connections (semantic similarities cross-community), Suggested Questions (lo que el grafo está mejor posicionado para responder).
- **`graph.html`** (200-500 KB): interactivo Pyvis sobre WebGL. Sin servidor — abrir directamente en el navegador. Útil para **exploración humana**, no para que el agente lo consuma. Si tu grafo tiene >5000 nodos, Graphify avisa antes de generar (puede explotar el browser).
- **Vault Obsidian** (opt-in con `--obsidian --obsidian-dir <path>`): un fichero markdown por nodo, agrupados por comunidad. Útil si ya usas Obsidian como vault personal y quieres tener el grafo *navegable como notas*. **Cuidado**: si lo apuntas a tu vault humano sin namespace, mete cientos de notas y enturbia el grafo manual. Usa subcarpeta dedicada (`graphify/<proyecto>/`).
- **Servidor MCP** (`graphify --mcp`): expone tools (`query_graph`, `get_node`, `shortest_path`, `god_nodes`) por stdio. Cualquier agente compatible (Claude Desktop, Cursor, agentes custom) lo consume. Slide 10 desarrolla.

## Cross-refs wiki

- `wiki/conceptos/mcp.md` — relevante para el output MCP.
- `wiki/herramientas/graphify.md` — listado completo de outputs y su uso.
- `wiki/proyectos/taller-llm-wiki.md` — el otro taller, donde el vault Obsidian es el formato principal.

## Anécdotas posibles

- En el experimento sobre el wiki, el output más rentable fue **`GRAPH_REPORT.md`** (lectura humana de god nodes y surprising connections). El `graph.json` no se consumió mucho porque las queries útiles para ese wiki ya las cubría el MOC manual.
- El vault Obsidian generado (597 notas en el primer pase) es **decorativo** si ya tienes un vault propio bien estructurado. Sirve más como auditoría que como reemplazo.

## Preguntas tipo

- *"¿Versionar `graph.json` en git?"* → Depende. Es regenerable, pero re-generarlo cuesta tokens. Convención razonable: lo metes en git si tu equipo lo consume vía MCP; lo gitignoreas si solo lo usas tú localmente y lo regeneras cuando hace falta.
- *"¿El HTML es seguro abrirlo de un compañero?"* → Sí, es estático sin servidor. Solo HTML+JS+datos embedded. Pero **si tu grafo contenía secrets** (paths, nombres internos), el HTML los expone igual que el JSON.

## Transición

"5 outputs distintos no significan 5 herramientas distintas. La pregunta real es: ¿cuándo Graphify es la elección correcta y cuándo otra herramienta gana?"
