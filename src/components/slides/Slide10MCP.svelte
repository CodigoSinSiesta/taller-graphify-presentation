<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const tools = [
    'query_graph',
    'get_node',
    'get_neighbors',
    'get_community',
    'god_nodes',
    'graph_stats',
    'shortest_path'
  ];

  const config = `{
  "mcpServers": {
    "graphify": {
      "command": "python3",
      "args": [
        "-m", "graphify.serve",
        "/abs/path/to/graphify-out/graph.json"
      ]
    }
  }
}`;
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">MCP y agentes externos</span>
      <h2>Tu grafo se vuelve <span class="highlight">infraestructura compartida</span></h2>
      <p class="lead">
        <code>graphify --mcp</code> levanta un servidor stdio. Cualquier agente compatible (Claude Desktop, Cursor, agentes custom) consume tu grafo sin tener Graphify instalado.
      </p>
    </div>

    <div class="grid">
      <div class="card-glass">
        <h3>Tools expuestas vía MCP</h3>
        <ul class="tools-list">
          {#each tools as tool}
            <li><code>{tool}</code></li>
          {/each}
        </ul>
        <p class="note">Read-only. Para modificar el grafo: <code>--update</code> manual desde el host.</p>
      </div>

      <div class="card-glass">
        <h3>Config en Claude Desktop</h3>
        <pre><code>{config}</code></pre>
      </div>
    </div>

    <div class="case-card card-glass">
      <h3>Caso real</h3>
      <p>
        Un equipo indexa su monorepo con Graphify, levanta el MCP server en CI, y todos los devs apuntan sus IDEs al mismo grafo.
        <strong>Cero indexación local</strong>. Coste: un re-pase nocturno (<code>graphify --update</code>) en CI ≈ <code>$0,20/día</code>.
      </p>
    </div>

    <p class="warning">
      ⚠️ <strong>Trade-off de saturación</strong>: si conectas 10 MCP servers a la vez, las descripciones de tools llenan el context window. El <em>instruction budget</em> se revienta de otra forma. Activa solo los relevantes por sesión.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 60% 20%, rgba(167,139,250,0.10) 0%, transparent 55%), radial-gradient(ellipse at 40% 80%, rgba(59,130,246,0.10) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.15rem); opacity: 0.85; margin: 0; line-height: 1.65; }
  .lead code { font-family: var(--font-mono); padding: 2px 6px; background: rgba(96,165,250,0.1); border-radius: 4px; color: var(--color-electric); }
  .grid { display: grid; grid-template-columns: 1fr 1fr; gap: var(--spacing-lg); align-items: stretch; }
  .card-glass { padding: var(--spacing-xl); display: flex; flex-direction: column; gap: var(--spacing-md); }
  h3 { margin: 0; font-size: 1.1rem; color: var(--color-electric); font-family: var(--font-mono); letter-spacing: 0.05em; }
  .tools-list { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: var(--spacing-xs); }
  .tools-list li code { font-family: var(--font-mono); padding: 6px 12px; background: rgba(96,165,250,0.08); border: 1px solid rgba(96,165,250,0.15); border-radius: var(--radius-sm); color: var(--color-electric); font-size: 0.9rem; display: block; }
  .note { font-size: 0.85rem; opacity: 0.7; font-style: italic; margin: 0; }
  .note code { font-family: var(--font-mono); padding: 1px 5px; background: rgba(96,165,250,0.1); border-radius: 3px; }
  pre { margin: 0; padding: var(--spacing-md); background: rgba(10,22,40,0.6); border-radius: var(--radius-sm); border: 1px solid rgba(96,165,250,0.1); overflow-x: auto; flex: 1; }
  pre code { font-family: var(--font-mono); font-size: 0.78rem; color: var(--color-electric); white-space: pre; }
  .case-card { padding: var(--spacing-lg); border-left: 3px solid #4ade80; }
  .case-card p { margin: 0; opacity: 0.9; line-height: 1.65; font-size: 0.95rem; }
  .case-card code { font-family: var(--font-mono); padding: 2px 6px; background: rgba(34,197,94,0.1); border-radius: 4px; color: #4ade80; font-size: 0.85rem; }
  .warning { padding: var(--spacing-md) var(--spacing-lg); background: rgba(251,146,60,0.08); border-left: 3px solid #fb923c; border-radius: var(--radius-sm); font-size: 0.95rem; line-height: 1.65; opacity: 0.9; margin: 0; }
  @media (max-width: 768px) { .grid { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } pre code { font-size: 0.7rem; } }
</style>
