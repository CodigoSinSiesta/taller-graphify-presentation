# Slide 8 — Hands-on 1: Setup + primera ejecución ⭐

## Tesis

En 25-30 minutos, cada asistente acaba con `graph.json`, `GRAPH_REPORT.md` y `graph.html` reales sobre el starter — y un primer ojo entrenado para leer la salida. **El output del hands-on no es la herramienta funcionando: es la lectura crítica del reporte.**

## Estructura del slide

Se renderiza como **lista de pasos numerados con checkboxes** (formato canónico de los slides hands-on de la organización). Para cada paso: comando exacto, tiempo estimado, criterio de éxito.

## Pasos (resumen visual)

1. **Verifica prerequisitos** (2 min) — Node ≥ 20, `uv` instalado, agente con filesystem.
2. **Instala Graphify** (3 min) — `uv tool install graphifyy && graphify --help`.
3. **Instala el starter** (5 min) — `git clone … && npm install && npm test && npm run typecheck`.
4. **Predice antes de ejecutar** (3 min) — escribe en papel: god node esperado, 2-3 comunidades esperadas, 1 conexión cross-modal esperada.
5. **Ejecuta el pipeline** (10-15 min) — `/graphify .` desde Claude Code. ~$0.30-0.50, ~250-350k tokens.
6. **Lee la salida** (5-7 min) — `GRAPH_REPORT.md` (god nodes, surprising), `graph.html` en navegador, `graph.json` solo `head`.
7. **Compara con tu predicción** (3 min) — ¿acertaste? ¿descubriste algo? ¿qué fue ruido del extractor?

## Mensajes clave para el ponente

- **El paso 4 (predecir) es no-negociable**. Si los asistentes saltan a ejecutar sin predecir, pierden el músculo crítico que el taller entrena. Insiste: "papel y boli, lo que esperáis ver, antes de tirar Graphify". Sin esto el slide 8 no rinde.
- **Mientras corre el pipeline (paso 5)**, gestiona el silencio: pide que **no toquen el filesystem** (los subagentes leen y escriben chunks parciales), y aprovecha para repasar la sección de "honesty rules" del slide 4 o anticipar el slide 9.
- **El paso 6 es donde la gente se pierde**. Lleva la lectura tú mismo: en directo, abre `GRAPH_REPORT.md` de tu propia ejecución de demostración, marca el god node #1, lee una surprising connection en voz alta, juzga si te parece útil o ruido. El asistente replica con su propio reporte.

## Predicciones esperadas sobre el starter

Sin spoilers en el slide, pero el ponente debe saberlas:

- **God node más probable**: `types.ts` (casi todo lo importa) o `tool-registry.ts` (puente entre LLM y tools).
- **Comunidades probables**: (a) auth + server (perímetro HTTP), (b) chat + LLM + tools (loop del agente), (c) raw/ docs (cluster temático separado del código).
- **Cross-modal probable**: `tool-registry.ts` ↔ `01-anthropic-skills.md` (sección "tool registry y cómo el modelo elige una tool"). Si Graphify lo detecta, validar la capa 2 — si no, ese fallo es interesante de discutir.

## Cross-refs wiki

- `ejercicios/01-setup.md` del starter — el guion completo paso a paso.
- `wiki/herramientas/graphify.md` — sección "Instalación y comandos".

## Preguntas tipo durante el hands-on

- *"Mi pipeline lleva 10 minutos y no acaba"* → Probablemente está esperando un subagente bloqueado. Mira si tu agente tiene logs visibles. Si supera 15 min, abortar (Ctrl-C) y reintentar.
- *"Mi `GRAPH_REPORT.md` no tiene Surprising Connections"* → Ocurre si el grafo es muy denso o muy disperso. No es bug; el algoritmo no encontró outliers de cohesion. Más material en `raw/` lo cambia.
- *"¿Cuánto pago si re-ejecuto?"* → `--update` solo re-procesa lo cambiado vía cache SHA256 — fracción del primer pase. Slide 11 desarrolla.

## Transición

"Ya tienes el grafo. Ahora la pregunta interesante: ¿cómo lo consultas en el día a día?"
