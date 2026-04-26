<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  // V4 TerminalSlide pattern (cf. css-slides-extra.jsx en el handoff de Claude Design)
  const lines: { type: 'cmd' | 'out' | 'err' | 'ok' | 'comment'; text: string }[] = [
    { type: 'comment', text: 'verifica prerequisitos' },
    { type: 'cmd', text: 'node --version && uv --version' },
    { type: 'out', text: 'v20.11.1' },
    { type: 'out', text: 'uv 0.4.15' },
    { type: 'comment', text: 'instala graphify' },
    { type: 'cmd', text: 'uv tool install graphifyy' },
    { type: 'ok',  text: '✓ installed graphifyy 0.8.2' },
    { type: 'comment', text: 'clona el starter y verifica' },
    { type: 'cmd', text: 'gh repo clone CodigoSinSiesta/taller-graphify-starter' },
    { type: 'cmd', text: 'cd taller-graphify-starter && npm install' },
    { type: 'cmd', text: 'npm test && npm run typecheck' },
    { type: 'ok',  text: '✓ 9 tests passed' },
    { type: 'ok',  text: '✓ tsc --noEmit (0 errors)' },
    { type: 'comment', text: 'ejecuta el pipeline completo desde Claude Code' },
    { type: 'cmd', text: '/graphify .' },
    { type: 'out', text: 'detect: 24 files · 6,840 words' },
    { type: 'out', text: 'dispatching 4 subagents in parallel...' },
    { type: 'out', text: 'merge: 138 nodes · 217 edges · 6 communities' },
    { type: 'ok',  text: '✓ graph.json + GRAPH_REPORT.md + graph.html' }
  ];

  const steps = [
    'Prerequisitos: Node ≥ 20 y uv. Si te falta uv, `curl -LsSf https://astral.sh/uv/install.sh | sh`.',
    'Instala Graphify globalmente. El package en PyPI es `graphifyy` con doble y; el binario es `graphify`.',
    'Clona el starter. Verifica con tests + typecheck antes de tocar Graphify.',
    '**Predice antes de ejecutar** — papel y boli: god node esperado, 2-3 comunidades, 1 conexión cross-modal.',
    'Lanza el pipeline desde Claude Code. ~$0.30-0.50, ~250-350k tokens, 4-5 subagentes en paralelo.',
    'Lee `GRAPH_REPORT.md` → `graph.html` → contrasta con tu predicción. Esa lectura es el output real.'
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label" data-accent="ok">Hands-on 1 · 25-30 min ⭐</span>
      <h2>Setup + <span class="highlight">primera ejecución</span> sobre el starter</h2>
      <p class="lead">
        Output del hands-on: <code>graph.json</code> real, reporte leído, y <strong>ojo entrenado</strong> para distinguir lo que descubrió Graphify vs lo que esperabas.
      </p>
    </div>

    <div class="terminal-row">
      <!-- Terminal window estilo Mac -->
      <div class="terminal" role="img" aria-label="Terminal demo del setup">
        <div class="terminal-titlebar">
          <span class="dot dot-r"></span>
          <span class="dot dot-y"></span>
          <span class="dot dot-g"></span>
          <span class="terminal-title">~/taller-graphify-starter · zsh</span>
        </div>
        <div class="terminal-body">
          {#each lines as l, i}
            {#if l.type === 'cmd'}
              <div class="ln"><span class="prompt-arrow">➜</span> <span class="prompt-tilde">~</span> <span class="cmd-text">{l.text}</span></div>
            {:else if l.type === 'out'}
              <div class="ln out">{l.text}</div>
            {:else if l.type === 'ok'}
              <div class="ln ok">{l.text}</div>
            {:else if l.type === 'err'}
              <div class="ln err">{l.text}</div>
            {:else}
              <div class="ln comment"># {l.text}</div>
            {/if}
          {/each}
          <div class="ln"><span class="prompt-arrow">➜</span> <span class="prompt-tilde">~</span> <span class="cursor"></span></div>
        </div>
      </div>

      <!-- Narración paso a paso -->
      <div class="narration">
        <div class="narration-label">narración paso a paso</div>
        {#each steps as step, i}
          <div class="step">
            <div class="step-num">{i + 1}</div>
            <div class="step-text">{@html step.replaceAll('**', '<strong>').replaceAll('`', '<code>')}</div>
          </div>
        {/each}
        <div class="pro-tip">
          <span class="tip-icon">💡</span>
          <div>
            <div class="tip-title">Pro tip</div>
            <div class="tip-body">El paso 4 (predecir) es no-negociable. Si saltas a ejecutar, pierdes el músculo crítico que el taller entrena.</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 80% 50%, rgba(34,197,94,0.06) 0%, transparent 55%), radial-gradient(ellipse at 20% 50%, rgba(59,130,246,0.10) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1280px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: #4ade80; letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(1.8rem, 4.5vw, 3rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-electrico), var(--color-cielo)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(0.95rem, 1.4vw, 1.05rem); opacity: 0.85; margin: 0; line-height: 1.6; max-width: 820px; }
  .lead code { font-family: var(--font-mono); padding: 2px 6px; background: rgba(96,165,250,0.1); border-radius: 4px; color: var(--color-cielo); }

  .terminal-row { display: grid; grid-template-columns: 1.6fr 1fr; gap: var(--spacing-xl); align-items: stretch; }

  /* Mac-style terminal */
  .terminal {
    background: #000;
    border: 1px solid var(--color-borde);
    border-radius: 10px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    box-shadow: 0 12px 40px rgba(0,0,0,0.4);
    min-height: 480px;
  }
  .terminal-titlebar {
    background: #161b22;
    padding: 10px 14px;
    border-bottom: 1px solid var(--color-borde);
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .dot { width: 11px; height: 11px; border-radius: 50%; flex-shrink: 0; }
  .dot-r { background: #ef4444; }
  .dot-y { background: #f59e0b; }
  .dot-g { background: #10b981; }
  .terminal-title { flex: 1; text-align: center; font-family: var(--font-mono); font-size: 11px; color: var(--color-tinta4); }
  .terminal-body { padding: 18px 22px; font-family: var(--font-mono); font-size: 13px; line-height: 1.7; color: #e2e8f0; flex: 1; overflow: hidden; }
  .ln { white-space: pre; overflow-x: hidden; text-overflow: ellipsis; }
  .ln.out { color: #cbd5e1; }
  .ln.ok { color: var(--color-ok); }
  .ln.err { color: var(--color-err); }
  .ln.comment { color: var(--color-tinta4); font-style: italic; }
  .prompt-arrow { color: var(--color-ok); }
  .prompt-tilde { color: var(--color-cielo); }
  .cmd-text { color: #e2e8f0; }
  .cursor { display: inline-block; width: 9px; height: 16px; background: var(--color-cielo); margin-left: 6px; vertical-align: middle; animation: blink 1s steps(2) infinite; }
  @keyframes blink { 50% { opacity: 0; } }

  /* Narración */
  .narration { display: flex; flex-direction: column; gap: 12px; }
  .narration-label { font-family: var(--font-mono); font-size: 11px; letter-spacing: 0.16em; text-transform: uppercase; color: var(--color-cielo); font-weight: 600; }
  .step { display: flex; gap: 12px; align-items: flex-start; }
  .step-num { flex-shrink: 0; width: 28px; height: 28px; border-radius: 50%; background: rgba(59,130,246,0.13); color: var(--color-cielo); font-family: var(--font-mono); font-size: 12px; font-weight: 700; display: flex; align-items: center; justify-content: center; border: 1px solid rgba(59,130,246,0.4); }
  .step-text { font-size: 13px; color: var(--color-tinta2); line-height: 1.5; padding-top: 4px; }
  .step-text :global(code) { font-family: var(--font-mono); font-size: 11px; padding: 1px 5px; background: rgba(96,165,250,0.1); border-radius: 3px; color: var(--color-cielo); }
  .step-text :global(strong) { color: var(--color-tinta); }
  .pro-tip { margin-top: auto; display: flex; gap: 12px; padding: 14px 16px; background: rgba(96,165,250,0.10); border-left: 3px solid var(--color-electrico); border: 1px solid rgba(96,165,250,0.25); border-left-width: 3px; border-radius: 0 8px 8px 0; align-items: flex-start; }
  .tip-icon { font-size: 16px; line-height: 1.2; }
  .tip-title { font-weight: 700; color: var(--color-cielo); margin-bottom: 4px; font-size: 13px; }
  .tip-body { font-size: 13px; line-height: 1.5; color: var(--color-tinta2); }

  @media (max-width: 1024px) {
    .terminal-row { grid-template-columns: 1fr; }
    .terminal { min-height: 360px; }
  }
  @media (max-width: 768px) {
    h2 { font-size: clamp(1.6rem, 6vw, 2.2rem); }
    .terminal-body { font-size: 12px; padding: 14px 16px; }
  }
</style>
