<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const takeaways = [
    { num: 1, text: 'Graphify resuelve un problema específico: context budget reventado por re-leer ficheros sobre corpus heterogéneos. No es sustituto universal.' },
    { num: 2, text: 'El cuadrante manda. Antes de adoptar, verifica que ninguna de las 3 alternativas (GitNexus, vector RAG, agentic search) cubre tu caso mejor.' },
    { num: 3, text: 'AST es gratis, semántica cuesta. Las 4 capas tienen rentabilidad muy distinta. Conoce qué pagas.' },
    { num: 4, text: 'El grafo es activo persistente, no decorativo. Si solo abres el HTML al primer pase y no consultas vía skill ni MCP, has tirado dinero.' },
    { num: 5, text: 'Honestidad como ventaja: cada arista marcada EXTRACTED/INFERRED/AMBIGUOUS. Cohesion en números, no símbolos. Eso es la diferencia con un RAG ingenuo.' }
  ];

  const recursos = [
    { label: 'Repo del starter', url: 'github.com/CodigoSinSiesta/taller-graphify-starter' },
    { label: 'Repo de Graphify', url: 'github.com/safishamsi/graphify' },
    { label: 'Wiki: graphify', url: 'codigosinsiesta.github.io/wiki/graphify' },
    { label: 'GitNexus (alternativa código)', url: 'github.com/abhigyanpatwari/GitNexus' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Cierre</span>
      <h2>Cinco takeaways. <span class="highlight">Un siguiente paso</span> esta semana.</h2>
    </div>

    <div class="takeaways">
      {#each takeaways as t}
        <div class="card-glass takeaway">
          <span class="num">{t.num}</span>
          <p>{t.text}</p>
        </div>
      {/each}
    </div>

    <div class="grid">
      <div class="card-glass next-step">
        <h3>El siguiente paso accionable</h3>
        <p>Elige <strong>una</strong>, hazla esta semana:</p>
        <ul>
          <li><strong>Cuadrante dijo SÍ</strong>: ejecuta sobre tu repo más volátil. <code>graphify hook install</code>. Lee el reporte.</li>
          <li><strong>Cuadrante dijo NO</strong> pero tienes notas o vault personal: prueba un pase puntual. Datos antes que opinión.</li>
          <li><strong>Cuadrante dijo "depende"</strong>: aplica un lint determinístico antes de pagar Graphify. Muchos casos se resuelven gratis.</li>
        </ul>
      </div>

      <div class="card-glass recursos">
        <h3>Recursos</h3>
        <ul>
          {#each recursos as r}
            <li><span class="r-label">{r.label}</span><br/><code>{r.url}</code></li>
          {/each}
        </ul>
      </div>
    </div>

    <p class="closing">
      El veredicto razonado que tienes en el papel del Ejercicio 3 <strong>es el output real del taller</strong>. La herramienta es útil solo si entra a tu workflow con criterio.<br/>
      Gracias por venir, y nos vemos en <code>dev/Formaciones/</code>.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 50% 0%, rgba(59,130,246,0.12) 0%, transparent 60%), radial-gradient(ellipse at 30% 90%, rgba(167,139,250,0.10) 0%, transparent 55%), radial-gradient(ellipse at 70% 90%, rgba(34,197,94,0.06) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric), #a78bfa); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .takeaways { display: flex; flex-direction: column; gap: var(--spacing-sm); }
  .takeaway { padding: var(--spacing-md) var(--spacing-lg); display: flex; align-items: center; gap: var(--spacing-lg); }
  .num { font-family: var(--font-mono); font-size: 2rem; font-weight: 800; color: var(--color-accent-bright); line-height: 1; flex-shrink: 0; min-width: 40px; }
  .takeaway p { margin: 0; line-height: 1.55; opacity: 0.9; font-size: 0.95rem; }
  .grid { display: grid; grid-template-columns: 1.3fr 1fr; gap: var(--spacing-lg); }
  .card-glass { padding: var(--spacing-xl); }
  h3 { margin: 0 0 var(--spacing-md) 0; font-size: 1.1rem; color: var(--color-electric); font-family: var(--font-mono); letter-spacing: 0.05em; }
  ul { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: var(--spacing-sm); }
  li { line-height: 1.55; opacity: 0.9; font-size: 0.92rem; }
  .next-step p { margin: 0 0 var(--spacing-sm) 0; opacity: 0.85; }
  .next-step code { font-family: var(--font-mono); padding: 1px 5px; background: rgba(96,165,250,0.1); border-radius: 3px; color: var(--color-electric); font-size: 0.82rem; }
  .recursos li { padding-bottom: var(--spacing-xs); border-bottom: 1px solid rgba(96,165,250,0.1); }
  .recursos li:last-child { border-bottom: none; }
  .r-label { font-size: 0.85rem; opacity: 0.85; }
  .recursos code { font-family: var(--font-mono); font-size: 0.78rem; color: var(--color-electric); }
  .closing { padding: var(--spacing-lg); background: rgba(59,130,246,0.06); border-radius: var(--radius-md); text-align: center; line-height: 1.65; opacity: 0.9; margin: 0; font-size: 1rem; }
  .closing code { font-family: var(--font-mono); padding: 2px 6px; background: rgba(96,165,250,0.1); border-radius: 4px; color: var(--color-electric); font-size: 0.85rem; }
  @media (max-width: 768px) { .grid { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } .takeaway { gap: var(--spacing-md); } .num { font-size: 1.5rem; min-width: 32px; } }
</style>
