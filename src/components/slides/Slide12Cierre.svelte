<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  import SlideResources from '@codigosinsiesta/theme/slides/SlideResources.svelte';

  let slideElement: HTMLElement;
  onMount(() => {
    if (slideElement) animateSlideEntrance(slideElement);
  });

  const takeaways = [
    { num: 1, text: 'Graphify resuelve un problema específico: context budget reventado por re-leer ficheros sobre corpus heterogéneos.' },
    { num: 2, text: 'El cuadrante manda. Verifica que ninguna de las 3 alternativas (GitNexus, vector RAG, agentic search) cubre tu caso mejor.' },
    { num: 3, text: 'AST gratis, semántica cuesta. Las 4 capas tienen rentabilidad distinta. Conoce qué pagas.' },
    { num: 4, text: 'El grafo es activo persistente, no decorativo. Si solo abres el HTML al primer pase, has tirado dinero.' },
    { num: 5, text: 'Honestidad: cada arista EXTRACTED/INFERRED/AMBIGUOUS. Cohesion en números. Diferencia con un RAG ingenuo.' }
  ];

  const links = [
    { icon: '🐙', title: 'Repo del starter',         url: 'github.com/CodigoSinSiesta/taller-graphify-starter',     tag: 'template' },
    { icon: '🎬', title: 'Slides de este taller',    url: 'codigosinsiesta.github.io/taller-graphify-presentation', tag: 'deck' },
    { icon: '📦', title: 'Graphify · repo oficial',  url: 'github.com/safishamsi/graphify',                         tag: 'tool' },
    { icon: '📖', title: 'Wiki: graphify',           url: 'codigosinsiesta.github.io/wiki/graphify',                tag: 'docs' },
    { icon: '🧭', title: 'GitNexus (alternativa)',   url: 'github.com/abhigyanpatwari/GitNexus',                    tag: 'alt' }
  ];

  const qr = {
    title: 'Escanea para todo',
    url: 'codigosinsiesta.github.io/taller-graphify',
    size: 220
  };
</script>

<div bind:this={slideElement} style="display: contents;">
  <SlideResources
    eyebrow="Cierre"
    title="Cinco takeaways. Un siguiente paso esta semana."
    titleHighlight="Un siguiente paso"
    {takeaways}
    {links}
    {qr}
    closing="El veredicto razonado del Ejercicio 3 **es el output real del taller**. La herramienta es útil solo si entra a tu workflow con criterio. Gracias por venir."
  />
</div>
