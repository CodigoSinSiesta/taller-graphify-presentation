<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const steps = [
    { num: 0, time: '2 min', title: 'Verifica prerequisitos', cmd: 'node --version  # >=20\nuv --version' },
    { num: 1, time: '3 min', title: 'Instala Graphify', cmd: 'uv tool install graphifyy\ngraphify --help' },
    { num: 2, time: '5 min', title: 'Instala el starter', cmd: 'git clone CodigoSinSiesta/taller-graphify-starter\nnpm install && npm test && npm run typecheck' },
    { num: 3, time: '3 min', title: 'Predice antes de ejecutar', cmd: '# Papel y boli. Escribe:\n# 1) god node esperado\n# 2) 2-3 comunidades esperadas\n# 3) 1 conexión cross-modal esperada' },
    { num: 4, time: '10-15 min', title: 'Ejecuta el pipeline', cmd: '/graphify .\n# ~$0.30-0.50, ~250-350k tokens\n# 4-5 subagentes en paralelo' },
    { num: 5, time: '5-7 min', title: 'Lee la salida', cmd: 'open graphify-out/GRAPH_REPORT.md\nopen graphify-out/graph.html\n# god nodes, surprising connections, suggested questions' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Hands-on 1 · 25-30 min ⭐</span>
      <h2>Setup + <span class="highlight">primera ejecución</span> sobre el starter</h2>
      <p class="lead">
        Output del hands-on: <code>graph.json</code> real, reporte leído, y <strong>ojo entrenado</strong> para distinguir lo que descubrió Graphify vs lo que esperabas.
      </p>
    </div>

    <div class="steps">
      {#each steps as step}
        <div class="card-glass step">
          <div class="step-meta">
            <span class="step-num">{step.num}</span>
            <span class="step-time">{step.time}</span>
          </div>
          <div class="step-body">
            <h3>{step.title}</h3>
            <pre><code>{step.cmd}</code></pre>
          </div>
        </div>
      {/each}
    </div>

    <p class="footnote">
      <strong>El paso 3 (predecir) es no-negociable</strong>. Si saltas a ejecutar sin escribir tu predicción, pierdes el músculo crítico que el taller entrena.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 80% 50%, rgba(34,197,94,0.06) 0%, transparent 55%), radial-gradient(ellipse at 20% 50%, rgba(59,130,246,0.10) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: #4ade80; letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.4rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.5vw, 1.1rem); opacity: 0.85; margin: 0; line-height: 1.65; }
  .lead code { font-family: var(--font-mono); padding: 2px 6px; background: rgba(96,165,250,0.1); border-radius: 4px; color: var(--color-electric); }
  .steps { display: grid; grid-template-columns: 1fr 1fr; gap: var(--spacing-md); }
  .step { padding: var(--spacing-lg); display: flex; flex-direction: column; gap: var(--spacing-sm); }
  .step-meta { display: flex; align-items: baseline; justify-content: space-between; gap: var(--spacing-md); }
  .step-num { font-family: var(--font-mono); font-size: 1.8rem; font-weight: 800; color: var(--color-accent-bright); line-height: 1; }
  .step-time { font-family: var(--font-mono); font-size: 0.78rem; padding: 3px 10px; border-radius: 999px; background: rgba(34,197,94,0.12); color: #4ade80; border: 1px solid rgba(34,197,94,0.3); }
  h3 { margin: 0; font-size: 1rem; line-height: 1.3; }
  pre { margin: 0; padding: var(--spacing-md); background: rgba(10,22,40,0.6); border-radius: var(--radius-sm); border: 1px solid rgba(96,165,250,0.1); overflow-x: auto; }
  code { font-family: var(--font-mono); font-size: 0.78rem; color: var(--color-electric); white-space: pre; }
  .footnote { padding: var(--spacing-md) var(--spacing-lg); background: rgba(251,146,60,0.08); border-left: 3px solid #fb923c; border-radius: var(--radius-sm); font-size: 0.95rem; opacity: 0.9; margin: 0; }
  @media (max-width: 768px) { .steps { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.4rem); } code { font-size: 0.72rem; } }
</style>
