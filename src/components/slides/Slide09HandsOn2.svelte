<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const queries = [
    { mode: 'BFS', cmd: '/graphify query "qué conecta tool registry con la decisión del LLM"', expects: 'Camino chat → tool-registry → tool-grep, cruzado con docs cross-modal' },
    { mode: 'Path', cmd: '/graphify path "auth" "chat"', expects: '2-3 hops vía server.ts. Si "no path", el AST no enlazó middleware — lección práctica' },
    { mode: 'Explain', cmd: '/graphify explain "tool-registry"', expects: 'Resumen + vecinos + dónde se cita en docs. Útil para onboarding rápido' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Hands-on 2 · 20-25 min ⭐</span>
      <h2>Skill en Claude Code + <span class="highlight">queries reales</span></h2>
      <p class="lead">
        Pasar de "tengo un graph.json precioso" a "el agente lo consulta de forma natural mientras trabajo".
      </p>
    </div>

    <div class="install-card card-glass">
      <h3>Paso 1 · Instala la skill (3 min)</h3>
      <pre><code>graphify claude install     # CLAUDE.md + PreToolUse hook
graphify codex install      # AGENTS.md
graphify cursor install     # .cursor/rules/graphify.mdc
graphify opencode install   # AGENTS.md + plugin
graphify aider install      # AGENTS.md</code></pre>
    </div>

    <div class="queries-section">
      <h3>Paso 2 · Tres queries dirigidas (10 min)</h3>
      <div class="queries">
        {#each queries as q}
          <div class="card-glass query">
            <span class="mode">{q.mode}</span>
            <code class="cmd">{q.cmd}</code>
            <p class="expect"><strong>Esperado</strong>: {q.expects}</p>
          </div>
        {/each}
      </div>
    </div>

    <div class="contrast-card card-glass">
      <h3>Paso 3 · Contraste con <code>rg</code> puro (5 min)</h3>
      <p>
        Misma pregunta, dos vías. Anota: <strong>tiempo</strong> · <strong>tokens consumidos por el agente</strong> · <strong>calidad</strong> · <strong>ruido</strong>.
        Esa tabla es lo que te llevas — no Graphify funcionando, sino <em>opinión razonada</em> sobre cuándo el grafo gana.
      </p>
    </div>

    <p class="footnote">
      <strong>Save-result loop</strong>: <code>graphify save-result --question "..." --answer "..." --nodes A B</code> guarda Q&As cerradas como nodos del grafo. Eso es lo que diferencia un grafo persistente de un RAG sin estado.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 80% 30%, rgba(167,139,250,0.08) 0%, transparent 55%), radial-gradient(ellipse at 20% 70%, rgba(59,130,246,0.10) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: #4ade80; letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.4rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.5vw, 1.1rem); opacity: 0.85; margin: 0; }
  h3 { margin: 0 0 var(--spacing-md) 0; font-size: 1.1rem; color: var(--color-electric); font-family: var(--font-mono); letter-spacing: 0.05em; }
  .install-card, .contrast-card { padding: var(--spacing-lg); }
  pre { margin: 0; padding: var(--spacing-md); background: rgba(10,22,40,0.6); border-radius: var(--radius-sm); border: 1px solid rgba(96,165,250,0.1); overflow-x: auto; }
  pre code { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); white-space: pre; }
  .queries-section h3 { margin-bottom: var(--spacing-md); }
  .queries { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .query { padding: var(--spacing-lg); display: flex; flex-direction: column; gap: var(--spacing-sm); }
  .mode { font-family: var(--font-mono); font-size: 0.78rem; padding: 3px 10px; border-radius: 999px; background: rgba(96,165,250,0.15); color: var(--color-accent-bright); border: 1px solid rgba(96,165,250,0.3); align-self: flex-start; }
  .cmd { display: block; padding: var(--spacing-sm) var(--spacing-md); background: rgba(10,22,40,0.6); border-radius: var(--radius-sm); font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); }
  .expect { margin: 0; opacity: 0.85; line-height: 1.5; font-size: 0.9rem; }
  .contrast-card p { margin: 0; opacity: 0.85; line-height: 1.65; }
  .contrast-card code, .footnote code { font-family: var(--font-mono); padding: 2px 6px; background: rgba(96,165,250,0.1); border-radius: 4px; color: var(--color-electric); font-size: 0.85rem; }
  .footnote { padding: var(--spacing-md) var(--spacing-lg); background: rgba(167,139,250,0.08); border-left: 3px solid #a78bfa; border-radius: var(--radius-sm); font-size: 0.95rem; line-height: 1.65; opacity: 0.9; margin: 0; }
  @media (max-width: 768px) { h2 { font-size: clamp(1.8rem, 7vw, 2.4rem); } pre code { font-size: 0.75rem; } }
</style>
