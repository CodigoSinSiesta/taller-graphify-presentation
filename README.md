# taller-graphify-presentation

Slides hermanas del [taller-graphify-starter](https://github.com/CodigoSinSiesta/taller-graphify-starter). 12 slides, 90-120 min de taller (≈ 50% slides + 50% hands-on guiado).

## Estado

**Notas de ponente listas, build de slides pendiente.** Este repo contiene `guia/` con un fichero markdown por slide siguiendo el patrón canónico de Código Sin Siesta (tesis, mensajes clave, anécdotas, transiciones, cross-refs al wiki, preguntas tipo). El boilerplate visual (Astro 5 + Svelte 5 + Tailwind 4 + GSAP) se hereda de los repos hermanos:

- [taller-llm-wiki-presentation](https://github.com/CodigoSinSiesta/taller-llm-wiki-presentation) — patrón visual canónico de talleres.
- [coding-agents-presentation](https://github.com/CodigoSinSiesta/coding-agents-presentation) — referencia para `Slide06Hallucinations.svelte` (background + orbs + grid animado, `card-glass`, breakpoints 900/768/480/390).

Forma de proceder propuesta: clonar uno de los repos anteriores como base, sustituir el contenido de cada slide por lo de `guia/<slide>.md`, mantener el patrón visual.

## Estructura de slides

| # | Fichero | Tesis en una línea |
|---|---------|--------------------|
| 1 | [`guia/01-hero.md`](./guia/01-hero.md) | Bienvenida + framing del veredicto razonado como output. |
| 2 | [`guia/02-problema.md`](./guia/02-problema.md) | Releer 40 ficheros por sesión es el síntoma del context budget reventado. |
| 3 | [`guia/03-knowledge-graph.md`](./guia/03-knowledge-graph.md) | Knowledge graph para corpus heterogéneo: nodes + edges + triples. |
| 4 | [`guia/04-anatomia.md`](./guia/04-anatomia.md) | Las 4 capas de Graphify: AST + semántica + clustering + outputs. |
| 5 | [`guia/05-outputs.md`](./guia/05-outputs.md) | Cuál output sirve a quién (graph.json / HTML / Obsidian / MCP). |
| 6 | [`guia/06-cuadrante.md`](./guia/06-cuadrante.md) | Cuadrante de decisión: Graphify vs GitNexus vs vector RAG vs agentic. |
| 7 | [`guia/07-cuando-si-cuando-no.md`](./guia/07-cuando-si-cuando-no.md) | Caso real: por qué este wiki dijo NO con datos. |
| 8 | [`guia/08-hands-on-1.md`](./guia/08-hands-on-1.md) | Hands-on 1: setup + primera ejecución sobre el starter. |
| 9 | [`guia/09-hands-on-2.md`](./guia/09-hands-on-2.md) | Hands-on 2: skill + queries reales + comparación con `rg`. |
| 10 | [`guia/10-mcp-y-agentes.md`](./guia/10-mcp-y-agentes.md) | `--mcp` expone el grafo a otros agentes. |
| 11 | [`guia/11-mas-alla.md`](./guia/11-mas-alla.md) | Incremental, hooks git, vault Obsidian, save-result loop. |
| 12 | [`guia/12-cierre.md`](./guia/12-cierre.md) | 5 takeaways + recursos + siguiente paso accionable. |

## Patrón visual canónico

Los 12 slides siguen el patrón validado en otros repos de Código Sin Siesta:

- `slide-background` con orbs radiales animados.
- `slide-header` con `label` + `title` + `highlight`.
- `card-glass` semánticas como contenedor de bullets / tablas.
- Grid 2× / 3× responsivo.
- Breakpoints: 900 / 768 / 480 / 390.
- Tipografía: Inter (sans) + JetBrains Mono (code).

## Cómo arrancar dev

Cuando el boilerplate Astro esté instalado:

```bash
pnpm install
pnpm dev   # localhost:4322
pnpm build
```

## Despliegue

GitHub Pages desde `main` vía Action existente en los repos hermanos. URL esperada:
`https://codigosinsiesta.github.io/taller-graphify-presentation/`.

## Cross-refs

- [Página del taller en el wiki](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/proyectos/taller-graphify.md).
- [Página de Graphify en el wiki](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/herramientas/graphify.md) — incluye el experimento real sobre el propio wiki que es citado en el slide 7.
- [Repo del starter](https://github.com/CodigoSinSiesta/taller-graphify-starter).

## Licencia

MIT.
