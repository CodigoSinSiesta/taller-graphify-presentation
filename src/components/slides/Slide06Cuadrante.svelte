<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const tools = [
    { name: 'GitNexus', scope: 'Solo código', det: '100% AST', cost: 'Gratis', priv: 'Local', mcp: '✓' },
    { name: 'Graphify', scope: 'Código + docs + papers + imágenes + vídeo', det: 'AST + LLM', cost: '~$0.30-0.80', priv: 'LLM ve docs', mcp: '✓' },
    { name: 'Vector RAG', scope: 'Texto plano', det: 'Embeddings', cost: 'Embeddings + storage', priv: 'API ve chunks', mcp: 'Wrapper' },
    { name: 'Agentic search', scope: 'Filesystem', det: 'Heurístico', cost: 'Tokens del agente', priv: 'Local', mcp: 'n/a' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">El cuadrante de decisión ⭐</span>
      <h2>Graphify es <span class="highlight">una de cuatro</span> herramientas. Conoce las otras tres antes de adoptarla.</h2>
    </div>

    <div class="table-wrapper card-glass">
      <table>
        <thead>
          <tr><th>Herramienta</th><th>Scope</th><th>Determinismo</th><th>Coste</th><th>Privacidad</th><th>MCP</th></tr>
        </thead>
        <tbody>
          {#each tools as t}
            <tr class:highlight-row={t.name === 'Graphify'}>
              <td class="name">{t.name}</td>
              <td>{t.scope}</td>
              <td>{t.det}</td>
              <td>{t.cost}</td>
              <td>{t.priv}</td>
              <td class="mcp">{t.mcp}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>

    <div class="rules">
      <h3>Reglas prácticas</h3>
      <ol>
        <li><strong>¿Cabe tu corpus en &lt;30k tokens?</strong> → Agentic search puro. No indexes nada.</li>
        <li><strong>¿Solo código y privacidad alta?</strong> → GitNexus.</li>
        <li><strong>¿Heterogéneo y queries cross-document?</strong> → Graphify.</li>
        <li><strong>¿Corpus enorme y "encuéntrame parecido"?</strong> → Vector RAG.</li>
        <li><strong>¿Combinaciones?</strong> → Sí. Vector para "narrow", grafo para "navegar".</li>
      </ol>
    </div>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 30% 30%, rgba(59,130,246,0.10) 0%, transparent 55%), radial-gradient(ellipse at 70% 70%, rgba(167,139,250,0.10) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1280px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-2xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .table-wrapper { padding: var(--spacing-lg); overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
  th, td { padding: var(--spacing-md); text-align: left; border-bottom: 1px solid rgba(96,165,250,0.15); }
  th { font-family: var(--font-mono); font-size: 0.75rem; color: var(--color-electric); letter-spacing: 0.1em; text-transform: uppercase; opacity: 0.85; }
  tr:last-child td { border-bottom: none; }
  .name { font-weight: 700; color: var(--color-electric); font-family: var(--font-mono); }
  .highlight-row { background: rgba(59,130,246,0.06); }
  .highlight-row .name { color: var(--color-accent-bright); }
  .mcp { font-family: var(--font-mono); }
  .rules { padding: var(--spacing-xl); background: rgba(59,130,246,0.06); border-radius: var(--radius-md); border: 1px solid rgba(96,165,250,0.15); }
  .rules h3 { margin: 0 0 var(--spacing-md) 0; font-size: 1.2rem; color: var(--color-electric); font-family: var(--font-mono); letter-spacing: 0.05em; }
  .rules ol { margin: 0; padding-left: var(--spacing-xl); display: flex; flex-direction: column; gap: var(--spacing-sm); }
  .rules li { line-height: 1.6; opacity: 0.9; font-size: 0.95rem; }
  @media (max-width: 768px) { h2 { font-size: clamp(1.6rem, 7vw, 2.4rem); } table { font-size: 0.78rem; } th, td { padding: var(--spacing-sm); } }
</style>
