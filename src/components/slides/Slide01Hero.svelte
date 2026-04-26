<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="animated-grid"></div>

  <div class="slide-content">
    <div class="eyebrow">
      <span class="eyebrow-dot"></span>
      <span>Taller · Código Sin Siesta · 2026</span>
    </div>

    <h1 class="title">
      Taller <span class="highlight">Graphify</span><br />
      <span class="subtitle-inline">Indexa un repo o /raw, navega el grafo, decide con criterio</span>
    </h1>

    <p class="subtitle">
      Knowledge graph + skill en tu agente + cuadrante de decisión honesto.<br />
      90-120 min para salir con un veredicto razonado: adoptas Graphify o no — con datos.
    </p>

    <div class="tools-row">
      <span class="tool-badge">Graphify</span>
      <span class="tool-badge">Tree-sitter AST</span>
      <span class="tool-badge">Subagentes Claude</span>
      <span class="tool-badge">Leiden clustering</span>
      <span class="tool-badge">MCP server</span>
      <span class="tool-badge highlight-badge">Veredicto razonado</span>
    </div>

    <div class="author-card card-glass">
      <img src="https://www.gravatar.com/avatar/8461e0b8674185c1df153079e820504b?s=120" alt="Alejandro de la Fuente" class="author-avatar" />
      <div class="author-info">
        <p class="author-name">Alejandro de la Fuente</p>
        <p class="author-role">Technical Leader · NTT Data · GDNE</p>
        <div class="author-links">
          <a href="https://tellmealex.dev" target="_blank" rel="noopener noreferrer" class="author-link">🌐</a>
          <a href="https://github.com/TellMeAlex" target="_blank" rel="noopener noreferrer" class="author-link">🐙</a>
          <a href="https://es.linkedin.com/in/alejandro-de-la-fuente" target="_blank" rel="noopener noreferrer" class="author-link">💼</a>
        </div>
      </div>
    </div>
  </div>

  <div class="orb orb-1"></div>
  <div class="orb orb-2"></div>
  <div class="orb orb-3"></div>
</div>

<style>
  .swiper-slide { position: relative; width: 100%; height: 100vh; display: flex; align-items: center; justify-content: center; overflow: hidden; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 15% 60%, rgba(59,130,246,0.12) 0%, transparent 55%), radial-gradient(ellipse at 85% 30%, rgba(30,58,138,0.18) 0%, transparent 55%); z-index: 1; }
  .animated-grid { position: absolute; inset: 0; background-image: linear-gradient(rgba(96,165,250,0.07) 1px, transparent 1px), linear-gradient(90deg, rgba(96,165,250,0.07) 1px, transparent 1px); background-size: 60px 60px; z-index: 0; animation: moveGrid 25s linear infinite; }
  .slide-content { position: relative; z-index: 2; max-width: 1200px; width: 100%; min-height: 80vh; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; align-items: flex-start; justify-content: center; gap: var(--spacing-xl); }
  .eyebrow { display: flex; align-items: center; gap: var(--spacing-sm); font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; animation: fadeUp 0.6s ease-out backwards; }
  .eyebrow-dot { width: 8px; height: 8px; background: var(--color-accent-bright); border-radius: 50%; animation: pulse 2s ease-in-out infinite; box-shadow: 0 0 8px rgba(59,130,246,0.8); }
  .title { font-size: clamp(3rem, 8vw, 6.5rem); font-weight: 900; line-height: 1.05; color: var(--color-neutral-light); animation: fadeUp 0.7s ease-out 0.1s backwards; }
  .subtitle-inline { display: block; font-size: clamp(1rem, 2.2vw, 1.6rem); font-weight: 500; opacity: 0.85; margin-top: var(--spacing-sm); line-height: 1.3; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric), #a78bfa); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .subtitle { font-size: clamp(1.05rem, 2.1vw, 1.5rem); opacity: 0.8; line-height: 1.65; max-width: 780px; animation: fadeUp 0.7s ease-out 0.2s backwards; }
  .tools-row { display: flex; flex-wrap: wrap; gap: var(--spacing-sm); animation: fadeUp 0.7s ease-out 0.3s backwards; }
  .tool-badge { padding: var(--spacing-sm) var(--spacing-lg); font-family: var(--font-mono); font-size: 0.9rem; font-weight: 600; background: rgba(30,58,138,0.4); border: 1px solid rgba(96,165,250,0.25); border-radius: 999px; color: var(--color-electric); letter-spacing: 0.05em; backdrop-filter: blur(4px); }
  .highlight-badge { background: rgba(59,130,246,0.2); border-color: var(--color-accent-bright); color: var(--color-neutral-light); box-shadow: 0 0 12px rgba(59,130,246,0.3); }
  .author-card { display: flex; align-items: center; gap: var(--spacing-xl); padding: var(--spacing-xl) var(--spacing-2xl); animation: fadeUp 0.7s ease-out 0.4s backwards; }
  .author-avatar { width: 68px; height: 68px; border-radius: 50%; flex-shrink: 0; object-fit: cover; }
  .author-name { font-size: 1.25rem; font-weight: 700; margin-bottom: var(--spacing-xs); }
  .author-role { font-size: 1rem; color: var(--color-electric); opacity: 0.85; font-family: var(--font-mono); }
  .author-links { display: flex; gap: var(--spacing-md); margin-top: var(--spacing-sm); }
  .author-link { font-size: 1.2rem; text-decoration: none; opacity: 0.7; transition: opacity var(--transition-fast); }
  .author-link:hover { opacity: 1; }
  .orb { position: absolute; border-radius: 50%; filter: blur(80px); pointer-events: none; z-index: 0; }
  .orb-1 { width: 500px; height: 500px; background: rgba(59,130,246,0.08); top: -100px; right: -100px; animation: floatOrb 12s ease-in-out infinite; }
  .orb-2 { width: 300px; height: 300px; background: rgba(96,165,250,0.06); bottom: 50px; right: 200px; animation: floatOrb 9s ease-in-out infinite reverse; }
  .orb-3 { width: 200px; height: 200px; background: rgba(167,139,250,0.05); bottom: -50px; left: 100px; animation: floatOrb 15s ease-in-out infinite; }
  @keyframes fadeUp { from { opacity: 0; transform: translateY(28px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes moveGrid { 0% { transform: translate(0,0); } 100% { transform: translate(60px,60px); } }
  @keyframes pulse { 0%,100% { opacity: 1; transform: scale(1); } 50% { opacity: 0.6; transform: scale(0.85); } }
  @keyframes floatOrb { 0%,100% { transform: translateY(0) scale(1); } 50% { transform: translateY(-30px) scale(1.05); } }
  @media (max-width: 768px) { .slide-content { align-items: center; } .eyebrow { justify-content: center; } .title { font-size: clamp(2.5rem, 11vw, 4.5rem); text-align: center; width: 100%; } .subtitle { text-align: center; } .tools-row { justify-content: center; gap: var(--spacing-xs); } }
  @media (max-width: 480px) { .title { font-size: clamp(2.2rem, 11vw, 3.5rem); } .subtitle { font-size: 0.95rem; } .author-avatar { width: 42px; height: 42px; } .tool-badge { padding: 10px var(--spacing-md); min-height: 44px; } }
</style>
