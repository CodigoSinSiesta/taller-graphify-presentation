<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const sintomas = [
    { icon: '🌀', title: 'Releer 40 ficheros por sesión', desc: 'Cada vez que preguntas "qué depende de X", tu agente lee el corpus desde cero. ~20k tokens consumidos antes de empezar a responder.' },
    { icon: '🪫', title: 'Instruction budget reventado', desc: 'Modelos frontier siguen ~150-200 instrucciones por context. Sumas prompt + AGENTS.md + tools + skills y ya estás en 300+. El modelo atiende a medias.' },
    { icon: '🧊', title: 'Conocimiento que no acumula', desc: 'Cada sesión empieza de cero. Lo que descubriste ayer no llega a hoy. RAG sin estado no aprende.' },
    { icon: '🪜', title: 'RAG vectorial falla en cross-doc', desc: 'Vector mete chunks aleatorios. Si la respuesta vive *entre* dos documentos (X depende de Y), no la encuentra.' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">El problema</span>
      <h2>El context budget <span class="highlight">se rompe</span> antes de empezar</h2>
      <p class="lead">
        Cuatro síntomas reconocibles. Si te pasan los cuatro, este taller es para ti — y probablemente Graphify (o una alternativa) compensa.
      </p>
    </div>

    <div class="grid">
      {#each sintomas as item}
        <div class="card-glass card">
          <div class="icon">{item.icon}</div>
          <h3>{item.title}</h3>
          <p>{item.desc}</p>
        </div>
      {/each}
    </div>

    <p class="footnote">
      <strong>Datos del experimento real</strong> sobre el wiki de Código Sin Siesta:
      pasar de ~76k tokens por consulta a ~3,5k. <span class="metric">22-30× reducción</span>
      (los 71× del marketing son para corpus mucho más grandes).
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 80% 10%, rgba(59,130,246,0.10) 0%, transparent 55%), radial-gradient(ellipse at 10% 90%, rgba(30,58,138,0.14) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-2xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.2rem); opacity: 0.85; max-width: 780px; margin: 0; }
  .grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--spacing-lg); }
  .card { padding: var(--spacing-xl); display: flex; flex-direction: column; gap: var(--spacing-sm); }
  .icon { font-size: 2rem; }
  h3 { margin: 0; font-size: 1.25rem; }
  .card p { margin: 0; opacity: 0.8; line-height: 1.5; font-size: 0.95rem; }
  .footnote { margin-top: var(--spacing-md); padding: var(--spacing-lg); background: rgba(59,130,246,0.06); border-left: 3px solid var(--color-accent-bright); border-radius: var(--radius-sm); font-size: 0.95rem; line-height: 1.6; opacity: 0.9; }
  .metric { font-family: var(--font-mono); color: var(--color-accent-bright); font-weight: 700; }
  @media (max-width: 768px) { .grid { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } }
</style>
