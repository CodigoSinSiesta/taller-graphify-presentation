<script lang="ts">
  import { onMount } from 'svelte';
  import { animateSlideEntrance } from '@/utils/animations';
  let slideElement: HTMLElement;
  onMount(() => { animateSlideEntrance(slideElement); });

  const outputs = [
    { file: 'graph.json', size: '50-500 KB', consumer: 'Tu agente vía skill', when: 'Siempre. Es el activo persistente. Versionable, re-consultable indefinidamente.' },
    { file: 'GRAPH_REPORT.md', size: '10-30 KB', consumer: 'Tú, lectura humana', when: 'Justo después del primer pase. God nodes + surprising connections + suggested questions.' },
    { file: 'graph.html', size: '200-500 KB', consumer: 'Tú, exploración visual', when: 'Cuando el reporte te llama la atención sobre algo concreto. Pyvis sobre WebGL, sin servidor.' },
    { file: 'obsidian/', size: 'Vault entero', consumer: 'Tu vault si lo tienes', when: 'Opt-in con --obsidian. Cuidado: si apuntas a vault humano, mete cientos de notas.' },
    { file: 'MCP server', size: '0 KB (proceso)', consumer: 'Otros agentes', when: 'graphify --mcp. Stdio. Claude Desktop, Cursor, agentes custom lo consumen.' }
  ];
</script>

<div class="swiper-slide" bind:this={slideElement}>
  <div class="slide-background"></div>
  <div class="slide-content">
    <div class="slide-header">
      <span class="label">Outputs y para qué</span>
      <h2>Cinco outputs. <span class="highlight">Cada uno tiene un consumidor distinto.</span></h2>
      <p class="lead">
        La trampa frecuente: generar todo y solo mirar el HTML.
        El output que más rentabiliza el coste del primer pase es <code>graph.json</code> consumido vía skill.
      </p>
    </div>

    <div class="table-wrapper card-glass">
      <table>
        <thead>
          <tr><th>Output</th><th>Tamaño típico</th><th>Consumidor</th><th>Cuándo</th></tr>
        </thead>
        <tbody>
          {#each outputs as o}
            <tr>
              <td><code>{o.file}</code></td>
              <td class="size">{o.size}</td>
              <td>{o.consumer}</td>
              <td class="when">{o.when}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
</div>

<style>
  .swiper-slide { position: relative; min-height: 100vh; display: flex; align-items: flex-start; justify-content: center; overflow-y: auto; }
  .slide-background { position: absolute; inset: 0; background: radial-gradient(ellipse at 50% 0%, rgba(59,130,246,0.10) 0%, transparent 60%), radial-gradient(ellipse at 50% 100%, rgba(30,58,138,0.14) 0%, transparent 55%); z-index: 0; }
  .slide-content { position: relative; z-index: 1; max-width: 1200px; width: 100%; padding: var(--spacing-2xl) var(--spacing-content); display: flex; flex-direction: column; gap: var(--spacing-2xl); }
  .slide-header { display: flex; flex-direction: column; gap: var(--spacing-md); }
  .label { font-family: var(--font-mono); font-size: 0.85rem; color: var(--color-electric); letter-spacing: 0.12em; text-transform: uppercase; }
  h2 { margin: 0; font-size: clamp(2rem, 5vw, 3.6rem); line-height: 1.1; }
  .highlight { background: linear-gradient(135deg, var(--color-accent-bright), var(--color-electric)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .lead { font-size: clamp(1rem, 1.6vw, 1.2rem); opacity: 0.85; max-width: 880px; margin: 0; line-height: 1.6; }
  .lead code { font-family: var(--font-mono); padding: 2px 8px; background: rgba(96,165,250,0.1); border-radius: 4px; color: var(--color-electric); }
  .table-wrapper { padding: var(--spacing-lg); overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; font-size: 0.95rem; }
  th, td { padding: var(--spacing-md); text-align: left; border-bottom: 1px solid rgba(96,165,250,0.15); vertical-align: top; }
  th { font-family: var(--font-mono); font-size: 0.8rem; color: var(--color-electric); letter-spacing: 0.1em; text-transform: uppercase; opacity: 0.85; }
  tr:last-child td { border-bottom: none; }
  td code { font-family: var(--font-mono); color: var(--color-accent-bright); font-size: 0.9rem; }
  .size { font-family: var(--font-mono); font-size: 0.85rem; opacity: 0.7; white-space: nowrap; }
  .when { line-height: 1.5; opacity: 0.85; }
  @media (max-width: 768px) { h2 { font-size: clamp(1.8rem, 7vw, 2.6rem); } table { font-size: 0.85rem; } th, td { padding: var(--spacing-sm); } }
</style>
