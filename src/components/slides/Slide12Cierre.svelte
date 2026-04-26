<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  // Componentes V4 desde @codigosinsiesta/theme v0.2.0
  import Eyebrow from '@codigosinsiesta/theme/components/Eyebrow.svelte';
  import QRCode  from '@codigosinsiesta/theme/components/QRCode.svelte';

  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const takeaways = [
    { num: 1, text: 'Graphify resuelve un problema específico: context budget reventado por re-leer ficheros sobre corpus heterogéneos.' },
    { num: 2, text: 'El cuadrante manda. Verifica que ninguna de las 3 alternativas (GitNexus, vector RAG, agentic search) cubre tu caso mejor.' },
    { num: 3, text: 'AST gratis, semántica cuesta. Las 4 capas tienen rentabilidad distinta. Conoce qué pagas.' },
    { num: 4, text: 'El grafo es activo persistente, no decorativo. Si solo abres el HTML al primer pase, has tirado dinero.' },
    { num: 5, text: 'Honestidad: cada arista EXTRACTED/INFERRED/AMBIGUOUS. Cohesion en números. Diferencia con un RAG ingenuo.' }
  ];

  const links = [
    { icon: '🐙', title: 'Repo del starter',         url: 'github.com/CodigoSinSiesta/taller-graphify-starter',      tag: 'template' },
    { icon: '🎬', title: 'Slides de este taller',    url: 'codigosinsiesta.github.io/taller-graphify-presentation',  tag: 'deck' },
    { icon: '📦', title: 'Graphify · repo oficial',  url: 'github.com/safishamsi/graphify',                          tag: 'tool' },
    { icon: '📖', title: 'Wiki: graphify',           url: 'codigosinsiesta.github.io/wiki/graphify',                 tag: 'docs' },
    { icon: '🧭', title: 'GitNexus (alternativa)',   url: 'github.com/abhigyanpatwari/GitNexus',                     tag: 'alt' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <Eyebrow>Cierre</Eyebrow>
      <h2>Cinco takeaways. <span class="highlight">Un siguiente paso</span> esta semana.</h2>
    </div>

    <div class="takeaways">
      {#each takeaways as t}
        <div class="takeaway card-glass">
          <span class="num">{t.num}</span>
          <p>{t.text}</p>
        </div>
      {/each}
    </div>

    <div class="resources-grid">
      <div class="links-col">
        {#each links as l}
          <div class="link-card">
            <div class="link-icon">{l.icon}</div>
            <div class="link-body">
              <div class="link-title">{l.title}</div>
              <div class="link-url">{l.url}</div>
            </div>
            {#if l.tag}<div class="link-tag">{l.tag}</div>{/if}
          </div>
        {/each}
      </div>

      <div class="qr-col">
        <QRCode
          size={220}
          title="Escanea para todo"
          url="codigosinsiesta.github.io/taller-graphify"
        />
      </div>
    </div>

    <p class="closing">
      El veredicto razonado del Ejercicio 3 <strong>es el output real del taller</strong>.
      La herramienta es útil solo si entra a tu workflow con criterio.
      Gracias por venir.
    </p>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 50% 0%, rgba(59,130,246,0.12) 0%, transparent 60%), radial-gradient(ellipse at 30% 90%, rgba(167,139,250,0.10) 0%, transparent 55%), radial-gradient(ellipse at 70% 90%, rgba(34,197,94,0.06) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1280px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  h2 { margin: 0; font-size: clamp(1.8rem, 4.5vw, 3rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-electrico), var(--color-cielo), #a78bfa); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }

  .takeaways { display: grid; grid-template-columns: repeat(5, 1fr); gap: 12px; }
  .takeaway { padding: 14px 16px; display: flex; flex-direction: column; gap: 8px; }
  .num { font-family: var(--font-mono); font-size: 1.3rem; font-weight: 800; color: var(--color-electrico); line-height: 1; }
  .takeaway p { margin: 0; line-height: 1.45; opacity: 0.9; font-size: 0.82rem; }

  .resources-grid { display: grid; grid-template-columns: 1.4fr 1fr; gap: 32px; align-items: center; }
  .links-col { display: flex; flex-direction: column; gap: 10px; }
  .link-card {
    background: var(--color-fondo-elev);
    border: 1px solid var(--color-borde);
    border-radius: 10px;
    padding: 14px 18px;
    display: flex;
    gap: 14px;
    align-items: center;
  }
  .link-icon { font-size: 22px; flex-shrink: 0; }
  .link-body { flex: 1; min-width: 0; }
  .link-title { font-family: var(--font-display); font-weight: 700; font-size: 15px; color: var(--color-tinta); line-height: 1.2; }
  .link-url { font-family: var(--font-mono); font-size: 11px; color: var(--color-cielo); margin-top: 2px; }
  .link-tag { font-family: var(--font-mono); font-size: 10px; color: var(--color-tinta3); padding: 4px 10px; border-radius: 99px; border: 1px solid var(--color-borde); flex-shrink: 0; }

  .qr-col { display: flex; align-items: center; justify-content: center; }

  .closing { padding: 16px 20px; background: rgba(59,130,246,0.06); border-radius: 12px; text-align: center; line-height: 1.6; opacity: 0.9; margin: 0; font-size: 0.95rem; }

  @media (max-width: 1024px) {
    .resources-grid { grid-template-columns: 1fr; }
    .qr-col { order: -1; }
    .takeaways { grid-template-columns: repeat(2, 1fr); }
  }
  @media (max-width: 768px) {
    .takeaways { grid-template-columns: 1fr; }
    h2 { font-size: clamp(1.5rem, 6vw, 2rem); }
  }
</style>
