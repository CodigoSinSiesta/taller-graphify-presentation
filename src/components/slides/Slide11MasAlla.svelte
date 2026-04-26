<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const palancas = [
    { num: 1, title: 'Incremental con --update', cmd: 'graphify --update <path>', desc: 'Solo re-procesa ficheros modificados via cache SHA256. Code-only changes no llaman al LLM. Coste: fracción del primer pase.' },
    { num: 2, title: 'Hooks git para rebuild automático', cmd: 'graphify hook install', desc: 'Tras cada commit, AST + rebuild de graph.json. Doc/image changes no trigger (requieren --update manual).' },
    { num: 3, title: 'Vault Obsidian con namespace', cmd: 'graphify --obsidian-dir ~/vault/graphify/<proyecto>', desc: 'Mal patrón: apuntar a vault humano sin namespace. Buen patrón: subcarpeta dedicada para no enturbiar el grafo manual.' },
    { num: 4, title: 'Save-result = memoria del agente', cmd: 'graphify save-result --question --answer --nodes', desc: 'Cada Q&A cerrada se guarda. La próxima --update la incorpora como nodo nuevo. El grafo aprende.' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Más allá del primer pase</span>
      <h2>Cuatro palancas <span class="highlight">por ratio impacto/coste</span></h2>
      <p class="lead">
        El primer pase es caro. Los siguientes son baratos. Si solo haces el primer pase y nunca más, has tirado dinero.
      </p>
    </div>

    <div class="palancas">
      {#each palancas as p}
        <div class="card-glass palanca">
          <div class="palanca-head">
            <span class="num">{p.num}</span>
            <h3>{p.title}</h3>
          </div>
          <code class="cmd">{p.cmd}</code>
          <p>{p.desc}</p>
        </div>
      {/each}
    </div>

    <p class="footnote">
      <strong>El save-result loop es lo que diferencia un grafo de un RAG</strong>. Tu RAG no aprende. El grafo persistente con save-result sí. Es <em>acumulación</em>, no consumo.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 30% 20%, rgba(34,197,94,0.06) 0%, transparent 55%), radial-gradient(ellipse at 70% 80%, rgba(59,130,246,0.10) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.15rem); opacity: 0.85; margin: 0; line-height: 1.65; }
  .palancas { display: grid; grid-template-columns: 1fr 1fr; gap: var(--spacing-lg); }
  .palanca { padding: var(--spacing-lg); display: flex; flex-direction: column; gap: var(--spacing-sm); }
  .palanca-head { display: flex; align-items: center; gap: var(--spacing-md); }
  .num { font-family: var(--font-mono); font-size: 2rem; font-weight: 800; color: var(--color-accent-bright); line-height: 1; }
  h3 { margin: 0; font-size: 1.05rem; flex: 1; }
  .cmd { display: block; padding: var(--spacing-sm) var(--spacing-md); background: rgba(10,22,40,0.6); border-radius: var(--radius-sm); border: 1px solid rgba(96,165,250,0.1); font-family: var(--font-mono); font-size: 0.82rem; color: var(--color-electric); overflow-x: auto; }
  .palanca p { margin: 0; opacity: 0.85; line-height: 1.55; font-size: 0.92rem; }
  .footnote { padding: var(--spacing-md) var(--spacing-lg); background: rgba(167,139,250,0.08); border-left: 3px solid #a78bfa; border-radius: var(--radius-sm); font-size: 0.95rem; line-height: 1.65; opacity: 0.9; margin: 0; }
  @media (max-width: 768px) { .palancas { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } }
</style>
