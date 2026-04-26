# Slide 10 — MCP y agentes externos

## Tesis

`graphify --mcp` levanta un servidor stdio que expone el grafo como **tools MCP**. Cualquier agente compatible (Claude Desktop, Cursor, agentes custom, otros MCP-aware) consume tu grafo sin tener Graphify instalado. **El grafo se vuelve infraestructura compartida.**

## Mensajes clave

- **MCP = Model Context Protocol**. Repaso de 30 segundos: protocolo abierto que estandariza cómo los LLMs consumen tools/resources/prompts desde servidores externos. Cubierto en `wiki/conceptos/mcp.md`.
- **Tools que Graphify expone vía MCP**: `query_graph` (BFS sobre el grafo), `get_node`, `get_neighbors`, `get_community`, `god_nodes`, `graph_stats`, `shortest_path`. **No expone**: write tools (no se modifica el grafo desde fuera) — eso lo decides tú con `--update`.
- **Configuración Claude Desktop**: snippet de `claude_desktop_config.json`:
  ```json
  {
    "mcpServers": {
      "graphify": {
        "command": "python3",
        "args": ["-m", "graphify.serve", "/abs/path/to/graphify-out/graph.json"]
      }
    }
  }
  ```
- **Caso de uso típico**: tu equipo comparte un grafo del codebase principal. Un dev pregunta "qué módulos dependen del módulo de auth?" desde Cursor sin tener Graphify instalado, porque su Cursor está conectado al MCP server que su compañero levanta.
- **Trade-off de saturación**: si conectas 10 MCP servers a la vez (Graphify + GitHub + Linear + Slack + …), las descripciones de las tools llenan el context window. El [instruction budget](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/conceptos/instruction-budget.md) se revienta de otra forma. Activa solo los relevantes por sesión.

## Cross-refs wiki

- `wiki/conceptos/mcp.md` — el protocolo en detalle.
- `wiki/conceptos/governance-agentes.md` — agent sprawl + gateway central, lo siguiente cuando MCPs se multiplican.
- `wiki/herramientas/arcade.md` — la alternativa "tool runtime" para delegated agent authorization.

## Anécdotas posibles

- En la conferencia Coding Agents 2026, varios speakers convergen en MCP como **lingua franca** de la integración. Anthropic lo lanzó, pero Cursor, Continue, Cline y otros lo adoptaron rápido. La portabilidad es real.
- Caso real: un equipo que indexa su monorepo con Graphify, levanta el MCP server en CI y todos los devs apuntan sus IDEs a él. Cero indexación local. Coste: un re-pase nocturno (`graphify --update`) en CI ≈ $0.20/día.

## Preguntas tipo

- *"¿Puedo exponer el MCP server a internet?"* → Sí pero con cuidado. El servidor stdio nativo es local. Para remoto, hay que envolverlo (HTTP+SSE). Y entonces aparece auth/authz como tema serio. Para equipos pequeños lo razonable es **VPN o ssh tunnel**, no exponer al mundo.
- *"¿Y si el grafo cambia mientras un agente lo está consultando?"* → El servidor MCP mantiene el `graph.json` cargado en memoria. Re-cargar requiere reiniciar. Patrón razonable: rebuild del grafo en CI, restart del MCP server tras commits.
- *"¿Esto es lo mismo que GitHub Copilot extensions?"* → Conceptualmente parecido (un servidor expone capabilities a un agente), pero Copilot extensions es propietario de GitHub. MCP es abierto, multi-vendor.

## Transición

"Has consultado el grafo, lo has expuesto. La última pregunta práctica: cómo lo mantienes vivo entre commits."
