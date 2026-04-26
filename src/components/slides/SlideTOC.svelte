<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  // V4 TOCSlide pattern (cf. css-slides-extra.jsx, lineas 133-179)
  // Agrupado en 6 capítulos pedagógicos (no 1 por slide). Refleja el arco real del taller.
  const chapters = [
    { title: 'El problema',         body: 'Context budget reventado y RAG vectorial que falla cross-doc.', duration: '5 min' },
    { title: 'Knowledge graph',      body: 'Nodes + edges + triples. La idea Karpathy "librarian, not chatbot".', duration: '8 min' },
    { title: 'Anatomía + outputs',   body: 'Las 4 capas de Graphify y los 5 outputs persistentes.', duration: '10 min' },
    { title: 'Cuadrante de decisión', body: 'Graphify vs GitNexus vs vector RAG vs agentic puro.',  duration: '12 min' },
    { title: 'Hands-on',              body: 'Setup + primera ejecución, skill + queries reales.',   duration: '50 min' },
    { title: 'Más allá + cierre',    body: 'MCP, incremental, save-result loop. 5 takeaways y QR.', duration: '15 min' }
  ];

  // current=null → no destaca ninguno (vista global). Cambiar a un índice para resaltar progreso.
  const current: number | null = null;
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Agenda</span>
      <h2>Lo que vamos a recorrer en <span class="highlight">90 minutos</span></h2>
      <p class="lead">
        6 capítulos. La mitad del tiempo en hands-on real. El output del taller no es la herramienta funcionando — es <strong>tu veredicto razonado</strong>.
      </p>
    </div>

    <div class="chapters">
      {#each chapters as c, i}
        {@const isCurrent = current === i}
        {@const isPast = current != null && i < current}
        <div class="chapter" class:is-current={isCurrent} class:is-past={isPast}>
          <div class="ch-num">{String(i + 1).padStart(2, '0')}</div>
          <div class="ch-body">
            <div class="ch-title">{c.title}</div>
            <div class="ch-desc">{c.body}</div>
          </div>
          <div class="ch-duration">{c.duration}</div>
          {#if isPast}<div class="ch-check">✓</div>{/if}
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 50% 0%, rgba(59,130,246,0.12) 0%, transparent 60%), radial-gradient(ellipse at 50% 100%, rgba(30,58,138,0.14) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1280px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-cielo); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.4rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-electrico), var(--color-cielo)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.5vw, 1.1rem); opacity: 0.85; max-width: 820px; margin: 0; line-height: 1.6; }
  .lead strong { color: var(--color-tinta); }

  .chapters { display: grid; grid-template-columns: repeat(2, 1fr); gap: 14px; }
  .chapter {
    background: var(--color-fondo-elev);
    border: 1px solid var(--color-borde);
    border-radius: 10px;
    padding: 16px 20px;
    display: flex;
    gap: 14px;
    align-items: center;
    transition: border-color var(--transition-base), box-shadow var(--transition-base);
  }
  .chapter:hover {
    border-color: var(--color-electrico);
    box-shadow: 0 0 0 1px rgba(59,130,246,0.25);
  }

  .chapter.is-current {
    background: linear-gradient(135deg, var(--color-cobalto), var(--color-azul));
    border-color: var(--color-electrico);
    box-shadow: 0 0 0 1px var(--color-electrico), 0 12px 30px rgba(59,130,246,0.25);
  }
  .chapter.is-past { opacity: 0.5; }

  .ch-num {
    flex-shrink: 0;
    width: 44px;
    height: 44px;
    border-radius: 10px;
    background: rgba(51,65,85,0.5);
    color: var(--color-cielo);
    font-family: var(--font-display);
    font-weight: 700;
    font-size: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid var(--color-borde2);
  }
  .chapter.is-current .ch-num {
    background: rgba(255,255,255,0.20);
    color: #fff;
    border-color: rgba(255,255,255,0.30);
  }

  .ch-body { flex: 1; min-width: 0; }
  .ch-title { font-family: var(--font-display); font-weight: 700; font-size: 17px; color: var(--color-tinta); line-height: 1.2; }
  .ch-desc { font-size: 12px; color: var(--color-tinta3); margin-top: 3px; line-height: 1.45; }
  .chapter.is-current .ch-title { color: #fff; }
  .chapter.is-current .ch-desc { color: rgba(255,255,255,0.85); }

  .ch-duration {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--color-tinta3);
    border: 1px solid var(--color-borde);
    padding: 4px 10px;
    border-radius: 99px;
    flex-shrink: 0;
  }
  .chapter.is-current .ch-duration {
    color: rgba(255,255,255,0.85);
    border-color: rgba(255,255,255,0.30);
  }

  .ch-check { color: var(--color-ok); font-size: 18px; font-weight: 700; }

  @media (max-width: 768px) {
    .chapters { grid-template-columns: 1fr; }
    h2 { font-size: clamp(1.6rem, 6vw, 2.4rem); }
    .ch-num { width: 36px; height: 36px; font-size: 14px; }
    .ch-title { font-size: 15px; }
  }
</style>
