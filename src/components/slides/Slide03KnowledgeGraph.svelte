<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const primitives = [
    { name: 'Node', desc: 'Una entidad nombrada. Una página, una función, una clase, un concepto, una persona.', example: '`tool-registry.ts`, `Anthropic`, `MCP`' },
    { name: 'Edge', desc: 'Relación tipada entre dos nodos. Citation, import, depends_on, conceptually_related_to.', example: 'chat.ts —imports→ tool-registry.ts' },
    { name: 'Triple', desc: 'Sujeto-predicado-objeto. Una afirmación contextualizada. La forma básica de codificar conocimiento.', example: '"Graphify usa Tree-sitter para AST"' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Knowledge graph en 30 segundos</span>
      <h2>Solo tres primitivas. <span class="highlight">El grafo emerge</span> si tomas notas con cuidado.</h2>
      <p class="lead">
        Wikipedia, Google Knowledge Graph y este wiki personal son lo mismo en el fondo: nodos + aristas + triples.
      </p>
    </div>

    <div class="grid">
      {#each primitives as p}
        <div class="card-glass card">
          <h3>{p.name}</h3>
          <p>{p.desc}</p>
          <code class="example">{p.example}</code>
        </div>
      {/each}
    </div>

    <div class="quote-box card-glass">
      <p class="quote">"I don't want a chatbot. I want a librarian."</p>
      <p class="attribution">— Andrej Karpathy, 2026</p>
      <p class="explainer">
        Un chatbot es un oráculo (preguntas, contesta, olvida). Un bibliotecario <strong>conoce sus libros</strong>:
        sabe cuál cita a cuál, cuáles tratan el mismo tema con posturas distintas, cuáles son fundacionales.
        Eso es un knowledge graph aplicado a un agente.
      </p>
    </div>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 20% 20%, rgba(167,139,250,0.10) 0%, transparent 55%), radial-gradient(ellipse at 80% 80%, rgba(59,130,246,0.12) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-2xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.2rem); opacity: 0.85; max-width: 780px; margin: 0; }
  .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: var(--spacing-lg); }
  .card { padding: var(--spacing-xl); display: flex; flex-direction: column; gap: var(--spacing-md); }
  h3 { margin: 0; font-size: 1.5rem; color: var(--color-electric); font-family: var(--font-mono); }
  .card p { margin: 0; opacity: 0.85; line-height: 1.5; font-size: 0.95rem; }
  .example { font-family: var(--font-mono); font-size: 0.85rem; padding: var(--spacing-sm) var(--spacing-md); background: rgba(96,165,250,0.08); border-radius: var(--radius-sm); color: var(--color-electric); display: block; }
  .quote-box { padding: var(--spacing-2xl); display: flex; flex-direction: column; gap: var(--spacing-md); border-left: 4px solid var(--color-accent-bright); }
  .quote { font-size: clamp(1.3rem, 3vw, 1.8rem); font-style: italic; font-weight: 500; margin: 0; line-height: 1.4; }
  .attribution { font-family: var(--font-mono); color: var(--color-electric); font-size: 0.9rem; margin: 0; }
  .explainer { font-size: 1rem; opacity: 0.85; line-height: 1.65; margin: 0; }
  @media (max-width: 768px) { .grid { grid-template-columns: 1fr; } h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } }
</style>
