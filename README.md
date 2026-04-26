# taller-graphify-presentation

Slides hermanas del [taller-graphify-starter](https://github.com/CodigoSinSiesta/taller-graphify-starter). 12 slides, 90-120 min de taller (≈ 50% slides + 50% hands-on guiado).

**En vivo**: [codigosinsiesta.github.io/taller-graphify-presentation/](https://codigosinsiesta.github.io/taller-graphify-presentation/)

## Estructura de slides

| # | Slide | Tesis en una línea |
|---|-------|--------------------|
| 1 | Hero | Bienvenida + framing del veredicto razonado como output. |
| 2 | El problema | Releer 40 ficheros por sesión es el síntoma del context budget reventado. |
| 3 | Knowledge graph | Knowledge graph: nodes + edges + triples. La idea Karpathy. |
| 4 | Anatomía | Las 4 capas de Graphify: AST + semántica + clustering + outputs. |
| 5 | Outputs | Cuál output sirve a quién (graph.json / HTML / Obsidian / MCP). |
| 6 | Cuadrante | Decisión: Graphify vs GitNexus vs vector RAG vs agentic search. |
| 7 | Cuándo SÍ/NO | Caso real: por qué este wiki dijo NO con datos. |
| 8 | Hands-on 1 | Setup + primera ejecución sobre el starter. |
| 9 | Hands-on 2 | Skill + queries reales + comparación con `rg`. |
| 10 | MCP | `--mcp` expone el grafo a otros agentes. |
| 11 | Más allá | Incremental, hooks git, vault Obsidian, save-result loop. |
| 12 | Cierre | 5 takeaways + recursos + siguiente paso accionable. |

Las **notas del ponente** (tesis, mensajes clave, anécdotas, transiciones, cross-refs al wiki, preguntas tipo) viven en [`guia/`](./guia/) — un fichero markdown por slide.

## Stack

- **Astro 5** + **Svelte 5** + **Tailwind CSS 4** + **GSAP** + TypeScript estricto.
- Patrón visual canónico de Código Sin Siesta (background + orbs + grid animado, `card-glass`, breakpoints 900/768/480/390) heredado de [taller-llm-wiki-presentation](https://github.com/CodigoSinSiesta/taller-llm-wiki-presentation).
- Despliegue: GitHub Pages desde `main` vía Action en `.github/workflows/deploy.yml`.

## Cómo arrancar dev

```bash
pnpm install
pnpm dev      # localhost:4328
pnpm build    # astro check && astro build → dist/
pnpm preview
```

**Requisitos**: Node.js ≥ 20, pnpm.

## Cross-refs

- [Página del taller en el wiki](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/proyectos/taller-graphify.md).
- [Página de Graphify en el wiki](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/herramientas/graphify.md) — incluye el experimento real sobre el propio wiki que es citado en el slide 7.
- [Repo del starter](https://github.com/CodigoSinSiesta/taller-graphify-starter).

## Licencia

MIT.
