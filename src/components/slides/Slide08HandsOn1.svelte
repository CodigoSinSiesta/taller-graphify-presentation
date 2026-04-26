<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  import SlideTerminal from '@codigosinsiesta/theme/slides/SlideTerminal.svelte';

  let slideElement: HTMLElement;
  // SlideTerminal renderiza su propio .swiper-slide; engancho la animación al primer .swiper-slide del DOM tras montar.
  onMount(() => {
    if (slideElement) animateSlideEntrance(slideElement);
  });

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
    'Prerequisitos: Node ≥ 20 y `uv`. Si te falta uv, `curl -LsSf https://astral.sh/uv/install.sh | sh`.',
    'Instala Graphify globalmente. El package en PyPI es `graphifyy` con doble y; el binario es `graphify`.',
    'Clona el starter. Verifica con tests + typecheck antes de tocar Graphify.',
    '**Predice antes de ejecutar** — papel y boli: god node esperado, 2-3 comunidades, 1 conexión cross-modal.',
    'Lanza el pipeline desde Claude Code. ~$0.30-0.50, ~250-350k tokens, 4-5 subagentes en paralelo.',
    'Lee `GRAPH_REPORT.md` → `graph.html` → contrasta con tu predicción. Esa lectura es el output real.'
  ];
</script>

<div bind:this={slideElement} style="display: contents;">
  <SlideTerminal
    eyebrow="Hands-on 1 · 25-30 min ⭐"
    eyebrowAccent="ok"
    title="Setup + primera ejecución sobre el starter"
    titleHighlight="primera ejecución"
    lead='Output del hands-on: <code>graph.json</code> real, reporte leído, y <strong>ojo entrenado</strong> para distinguir lo que descubrió Graphify vs lo que esperabas.'
    terminalTitle="~/taller-graphify-starter · zsh"
    {lines}
    {steps}
    tip="El paso 4 (predecir) es no-negociable. Si saltas a ejecutar, pierdes el músculo crítico que el taller entrena."
  />
</div>
