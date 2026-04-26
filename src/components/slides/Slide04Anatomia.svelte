<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const layers = [
    { num: 1, name: 'AST determinístico', cost: 'Gratis', tech: 'Tree-sitter (12+ lenguajes)', desc: 'Parsea código y extrae funciones, clases, métodos, imports. Sin LLM. Para repos puros, suficiente.' },
    { num: 2, name: 'Extracción semántica', cost: '~$0.30-0.80', tech: 'Subagentes Claude paralelos', desc: 'Procesa docs/PDFs/imágenes. Devuelve nodos+aristas con EXTRACTED/INFERRED/AMBIGUOUS y confidence_score. La capa cara y la única donde tu API key entra.' },
    { num: 3, name: 'Clustering Leiden', cost: 'Gratis', tech: 'NetworkX + Leiden', desc: 'Detecta comunidades temáticas. Identifica god nodes (más conectados) y surprising connections (alta similitud cross-community).' },
    { num: 4, name: 'Outputs persistentes', cost: '0 LLM (regeneración)', tech: 'JSON + HTML + Obsidian + MCP', desc: 'graph.json (consumible), GRAPH_REPORT.md (legible), graph.html (Pyvis interactivo), vault Obsidian opcional, servidor MCP. Vive entre sesiones.' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Anatomía de Graphify</span>
      <h2>Cuatro capas. Solo <span class="highlight">una cuesta tokens</span>.</h2>
      <p class="lead">
        Conocer qué pagas y por qué te hace usuario crítico, no promotor entusiasta.
      </p>
    </div>

    <div class="layers">
      {#each layers as layer}
        <div class="card-glass layer">
          <div class="layer-num">{layer.num}</div>
          <div class="layer-body">
            <div class="layer-head">
              <h3>{layer.name}</h3>
              <span class="cost" class:free={layer.cost === 'Gratis' || layer.cost.includes('0 LLM')}>{layer.cost}</span>
            </div>
            <p class="tech">{layer.tech}</p>
            <p class="desc">{layer.desc}</p>
          </div>
        </div>
      {/each}
    </div>

    <p class="footnote">
      <strong>Lección clave</strong>: en el experimento real sobre el wiki de Código Sin Siesta,
      las capas 1+3+4 son <em>regenerables casi gratis</em>. La capa 2 cuesta ~$0.50 por pase
      completo y es el único cuello de botella económico para iterar.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 70% 20%, rgba(59,130,246,0.10) 0%, transparent 55%), radial-gradient(ellipse at 30% 80%, rgba(30,58,138,0.16) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-2xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.2rem); opacity: 0.85; max-width: 780px; margin: 0; }
  .layers { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .layer { display: flex; gap: var(--spacing-xl); padding: var(--spacing-xl); align-items: flex-start; }
  .layer-num { font-family: var(--font-mono); font-size: 3rem; font-weight: 800; line-height: 1; color: var(--color-accent-bright); flex-shrink: 0; min-width: 60px; }
  .layer-body { flex: 1; display: flex; flex-direction: column; gap: var(--spacing-xs); }
  .layer-head { display: flex; align-items: baseline; justify-content: space-between; gap: var(--spacing-md); flex-wrap: wrap; }
  h3 { margin: 0; font-size: 1.4rem; }
  .cost { font-family: var(--font-mono); font-size: 0.9rem; padding: 4px 12px; border-radius: 999px; background: rgba(251,146,60,0.15); color: #fb923c; border: 1px solid rgba(251,146,60,0.3); }
  .cost.free { background: rgba(34,197,94,0.12); color: #4ade80; border-color: rgba(34,197,94,0.25); }
  .tech { margin: 0; font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); opacity: 0.85; }
  .desc { margin: 0; opacity: 0.85; line-height: 1.6; font-size: 0.95rem; }
  .footnote { padding: var(--spacing-lg); background: rgba(59,130,246,0.06); border-left: 3px solid var(--color-accent-bright); border-radius: var(--radius-sm); font-size: 0.95rem; line-height: 1.6; opacity: 0.9; }
  @media (max-width: 768px) { .layer { flex-direction: column; gap: var(--spacing-md); } .layer-num { font-size: 2.2rem; min-width: 0; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } }
</style>
