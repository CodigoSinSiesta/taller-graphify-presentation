<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const metrics = [
    { label: 'Nodos extraídos', value: '201' },
    { label: 'Aristas', value: '333' },
    { label: 'Comunidades', value: '16' },
    { label: 'Reducción tokens', value: '29,9×' },
    { label: 'Coste real', value: '~$0,50' },
    { label: 'Cohesion grandes', value: '0,05-0,07' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Cuándo SÍ / Cuándo NO (con datos reales)</span>
      <h2>Caso real: Graphify <span class="highlight">sobre este propio wiki</span> dijo NO.</h2>
      <p class="lead">
        85 docs markdown · 79.224 palabras · MOC manual + lint determinístico ya en su sitio.
      </p>
    </div>

    <div class="metrics">
      {#each metrics as m}
        <div class="card-glass metric-card">
          <p class="metric-value">{m.value}</p>
          <p class="metric-label">{m.label}</p>
        </div>
      {/each}
    </div>

    <div class="grid">
      <div class="card-glass yes">
        <h3>SÍ compensa</h3>
        <ul>
          <li>Codebase desconocido y volátil (>5 cambios/día)</li>
          <li>Corpus heterogéneo grande (papers + tweets + screenshots)</li>
          <li>Necesitas exponer estructura vía MCP a varios agentes</li>
          <li>Quieres detectar patrones cross-proyecto</li>
        </ul>
      </div>
      <div class="card-glass no">
        <h3>NO compensa</h3>
        <ul>
          <li>Wiki ya curado con MOC manual (este caso)</li>
          <li>Corpus pequeño donde <code>rg</code> + lectura cubre</li>
          <li>Repos puros de código → mejor GitNexus</li>
          <li>Preguntas únicas que no se repiten</li>
        </ul>
      </div>
    </div>

    <p class="footnote">
      <strong>Decisión razonada</strong>: NO adoptar como pipeline regular. Mantener para auditoría puntual cada >3× crecimiento (umbral: >200 ficheros / >150k palabras). El <em>lint determinístico</em> (gratis, segundos) detectó 130 asimetrías que Graphify no detecta. Para "encontrar enlaces faltantes", lint > Graphify.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 80% 20%, rgba(251,146,60,0.08) 0%, transparent 55%), radial-gradient(ellipse at 20% 80%, rgba(34,197,94,0.08) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.15rem); opacity: 0.85; margin: 0; }
  .metrics { display: grid; grid-template-columns: repeat(6, 1fr); gap: var(--spacing-md); }
  .metric-card { padding: var(--spacing-md); display: flex; flex-direction: column; gap: 2px; align-items: center; text-align: center; }
  .metric-value { font-family: var(--font-mono); font-size: 1.6rem; font-weight: 800; color: var(--color-accent-bright); margin: 0; }
  .metric-label { font-size: 0.75rem; opacity: 0.75; margin: 0; line-height: 1.3; }
  .grid { display: grid; grid-template-columns: 1fr 1fr; gap: var(--spacing-lg); }
  .card-glass.yes { padding: var(--spacing-xl); border-left: 3px solid #4ade80; }
  .card-glass.no { padding: var(--spacing-xl); border-left: 3px solid #fb923c; }
  .yes h3 { margin: 0 0 var(--spacing-md) 0; color: #4ade80; font-family: var(--font-mono); font-size: 1.1rem; letter-spacing: 0.05em; }
  .no h3 { margin: 0 0 var(--spacing-md) 0; color: #fb923c; font-family: var(--font-mono); font-size: 1.1rem; letter-spacing: 0.05em; }
  ul { margin: 0; padding-left: var(--spacing-lg); display: flex; flex-direction: column; gap: var(--spacing-xs); }
  li { line-height: 1.5; opacity: 0.85; font-size: 0.95rem; }
  code { font-family: var(--font-mono); padding: 1px 6px; background: rgba(96,165,250,0.1); border-radius: 3px; color: var(--color-electric); font-size: 0.85rem; }
  .footnote { padding: var(--spacing-lg); background: rgba(59,130,246,0.06); border-left: 3px solid var(--color-accent-bright); border-radius: var(--radius-sm); font-size: 0.95rem; line-height: 1.65; opacity: 0.9; margin: 0; }
  @media (max-width: 768px) { .metrics { grid-template-columns: repeat(3, 1fr); } .grid { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } }
</style>
