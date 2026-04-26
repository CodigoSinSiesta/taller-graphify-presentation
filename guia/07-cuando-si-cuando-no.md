# Slide 7 — Cuándo SÍ / Cuándo NO (con datos reales)

## Tesis

El experimento real de Graphify sobre el wiki personal de Código Sin Siesta dijo **NO adoptar como pipeline regular**. Y esa decisión, razonada con datos, es el output más útil del experimento. Aquí está la trazabilidad para que repliques el razonamiento.

## El caso de prueba

- Corpus: 85 docs markdown + 10 imágenes (los assets de la conferencia Coding Agents 2026).
- Total: 79.224 palabras.
- Curado a mano con MOC temático (`wiki/ia.md`), tags controlados, plantillas, lint asistido.
- Está bajo control de versiones (git), navegable con Obsidian Graph view.

## Resultados del segundo pase (2026-04-26)

- **201 nodos** post-dedup, 333 aristas, 16 comunidades detectadas.
- **29,9× reducción de tokens** por consulta vs releer el corpus completo (76k → 3.5k tokens).
- **5 surprising connections**: 4 ya estaban codificadas como wikilinks bidireccionales en el wiki manual; 1 era débil (descartable).
- **Coste real**: ~335k tokens / **~$0.40-0.60 USD** por pase completo.
- Cohesion 0.05-0.07 en comunidades grandes — el clustering Leiden agrupa cosas más heterogéneas que el MOC manual.

## La decisión razonada

**NO** adoptar como pipeline regular. Razones:

1. El MOC manual ya captura ~95% de las conexiones temáticas relevantes con cohesion superior.
2. El coste de re-pasar Graphify ($0.50) cada vez que el wiki cambia (varias veces por semana) acumula >$10/mes para un valor marginal sobre lo que ya tenemos.
3. El **lint determinístico** (gratis, segundos) detectó las 130 asimetrías de wikilinks que Graphify no detecta. Para "encontrar enlaces faltantes", lint > Graphify.
4. Las "surprising connections" del clustering eran 4/5 ya conocidas y 1/5 débil. Cero conocimiento nuevo accionable.

**SÍ** se mantiene Graphify para **auditoría puntual** cada >3× crecimiento del wiki (umbral: >200 ficheros / >150k palabras). Es decir: una vez al año si crece al ritmo actual.

## Cuándo SÍ compensa Graphify (criterios extraídos del cuadrante)

- Codebase desconocido y volátil (>5 cambios/día, >100 ficheros).
- Corpus heterogéneo grande (`raw/` con papers + tweets + screenshots + código mezclado).
- Necesitas exponer estructura a varios agentes vía MCP.
- Quieres detectar patrones cross-proyecto entre repos distintos.

## Cuándo NO compensa Graphify

- Wiki ya curado con MOC manual (este caso documentado).
- Corpus pequeño donde `rg` + lectura cubre.
- Repos puros de código (mejor [GitNexus](https://github.com/abhigyanpatwari/GitNexus)).
- Preguntas únicas que no se repiten (no se amortiza el indexado).

## Cross-refs wiki

- `wiki/herramientas/graphify.md` — sección *Experimento sobre este wiki (2026-04-25)*.
- `wiki/log.md` — entrada `[2026-04-26] lint | Limpieza masiva de asimetrías + broken links + segundo pase Graphify`.

## Anécdotas posibles

- "El primer pase me dio 22,3× ahorro y 4 surprising connections. Tres de ellas ya estaban como wikilinks. La cuarta era genuina y la apliqué. Total de conocimiento nuevo: 1 backlink. Coste: $0.50. Ratio: $0.50 por backlink. Inviable como herramienta de descubrimiento regular."
- "La lección no es que Graphify sea mala — es **diseñada para corpus volátiles y heterogéneos**. Aplicarla a un wiki curado y maduro es como usar un hacha para cortar pan. La culpa es del que lo aplica mal, no del hacha."

## Preguntas tipo

- *"¿Si mi repo crece, cuándo re-evaluar?"* → Cuando alguno de estos cambie significativamente: tamaño total ×3, frecuencia de cambios ×3, número de fuentes externas heterogéneas ×3, o cuando incorpores material no-markdown (PDFs, imágenes, transcripts).
- *"¿Y si yo no tengo MOC manual?"* → Entonces Graphify probablemente sí compensa. La lección de este experimento es **comparativa**: contra qué baseline lo evalúas. Sin MOC, el baseline es "no tener nada", y ahí Graphify gana.

## Transición

"Suficiente teoría. Vamos a hacerlo. El primer hands-on monta tu propio Graphify sobre el starter del taller."
