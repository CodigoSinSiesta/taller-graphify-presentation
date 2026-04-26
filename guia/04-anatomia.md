# Slide 4 — Anatomía de Graphify

## Tesis

Graphify tiene 4 capas que es útil distinguir porque la rentabilidad de cada una es muy distinta. **AST es gratis**, **clustering es gratis**, **semántica cuesta tokens** y los **outputs persistentes** son lo que justifica todo el coste.

## Mensajes clave

- **Capa 1 — AST determinístico** (gratis, instantáneo). Tree-sitter parsea código en 12+ lenguajes (TypeScript, Python, Java, Go, Rust, C++, Ruby…) y extrae funciones, clases, métodos, imports. Sin LLM. Para repos puros de código esto es **suficiente** y aquí podrías usar [GitNexus](https://github.com/abhigyanpatwari/GitNexus) en su lugar.
- **Capa 2 — Extracción semántica con subagentes** (~$0.30-0.80 por corpus medio). Para docs/PDFs/imágenes, dispatcha agentes Claude (o equivalente) en paralelo. Cada uno devuelve nodos + aristas con `EXTRACTED`/`INFERRED`/`AMBIGUOUS` y `confidence_score`. **Es la capa cara y la única donde tu API key entra**.
- **Capa 3 — Clustering Leiden + análisis** (gratis, ~10s). Sobre el grafo merged se detectan comunidades temáticas (algoritmo Leiden — moderno, rápido, modular). De aquí salen los **god nodes** (más conectados) y las **surprising connections** (alta similitud semántica entre comunidades distintas).
- **Capa 4 — Outputs persistentes**: `graph.json` (consumible), `GRAPH_REPORT.md` (legible), `graph.html` (interactivo Pyvis), vault Obsidian opcional, exports a Cypher/GraphML/SVG, servidor MCP via `--mcp`. Eso es lo que **vive entre sesiones** y justifica el coste del primer pase.

## Cross-refs wiki

- `wiki/herramientas/graphify.md` — descripción técnica completa.
- `wiki/conceptos/agentic-search-vs-rag.md` — la sección "Graph RAG" enmarca dónde encaja Graphify.
- `wiki/conceptos/agente-aprendiz.md` — el patrón save-result que cierra el loop con la capa 4.

## Anécdotas posibles

- En el experimento sobre este wiki: 4 subagentes en paralelo, cada uno procesando ~17 ficheros, 9 minutos totales. Sumando: ~335k tokens. Coste real: $0.40-0.60. La capa que paga es la 2.
- El AST de TypeScript con Tree-sitter es muy bueno: `tool-registry.ts` → `grepTool`, `readFileTool` queda capturado literalmente. Es la capa que **no se equivoca**. La 2 es donde aparecen los `INFERRED` y los falsos positivos.

## Preguntas tipo

- *"¿Puedo correr solo la capa 1 (AST)?"* → `graphify --update` cuando solo cambia código no llama a la capa 2. Y `cluster-only` re-corre solo la 3 sobre un grafo existente. Modular.
- *"¿La capa 2 manda mi código a una API externa?"* → Sí, a través del LLM que tu agente usa. Si tu código es propietario y sensible, el slide 6 menciona [GitNexus](https://github.com/abhigyanpatwari/GitNexus) como alternativa 100% local.
- *"¿Por qué Leiden y no Louvain?"* → Modular, más rápido, mejor en grafos con comunidades de tamaño desigual. Es un detalle de implementación que no afecta a tu uso, salvo que veas comunidades muy desbalanceadas.

## Transición

"Cada capa produce algo concreto. La pregunta práctica es: ¿qué output uso para qué?"
