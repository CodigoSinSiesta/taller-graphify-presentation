# Slide 12 — Cierre

## Tesis

5 takeaways accionables, recursos para profundizar, un siguiente paso concreto. **Si no haces el siguiente paso esta semana, el taller no ha rentabilizado el coste de los $0.50 que has pagado por el hands-on.**

## Los 5 takeaways

1. **Graphify resuelve un problema específico**: el context budget reventado por re-leer ficheros en cada sesión sobre corpus heterogéneos. No es un sustituto universal de tu setup actual.
2. **El cuadrante manda**. Antes de adoptar Graphify, verifica que ninguna de las 3 alternativas (GitNexus para código puro, vector RAG para corpus enorme y plano, agentic search puro para corpus pequeño) cubre tu caso mejor.
3. **AST es gratis, semántica cuesta**. Las 4 capas de Graphify tienen rentabilidad muy distinta. Conoce qué pagas y por qué.
4. **El grafo es activo persistente, no decorativo**. Si solo abres el HTML al primer pase y no consultas el `graph.json` ni vía skill ni vía MCP en las semanas siguientes, has tirado dinero.
5. **Honestidad como ventaja competitiva** del enfoque. Cada arista marcada `EXTRACTED` / `INFERRED` / `AMBIGUOUS`. El reporte expone token cost. Cohesion en números, no en símbolos. Eso es lo que diferencia Graphify de un RAG ingenuo, y es lo que justifica el coste.

## Recursos

- **Repo del starter**: [github.com/CodigoSinSiesta/taller-graphify-starter](https://github.com/CodigoSinSiesta/taller-graphify-starter) — vuelve cuando dudes sobre la pipeline.
- **Repo de Graphify**: [github.com/safishamsi/graphify](https://github.com/safishamsi/graphify) — los issues y los CHANGELOGs son donde se ve la evolución real.
- **Página de Graphify en el wiki**: [wiki/herramientas/graphify](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/herramientas/graphify.md) — incluye el experimento real sobre el wiki de Código Sin Siesta con números.
- **Página del taller**: [wiki/proyectos/taller-graphify](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/proyectos/taller-graphify.md).
- **GitNexus** (alternativa para código puro): [github.com/abhigyanpatwari/GitNexus](https://github.com/abhigyanpatwari/GitNexus).
- **Patrón LLM Wiki original** (Karpathy): [karpathy.bearblog.dev/llm-wiki](https://karpathy.bearblog.dev/llm-wiki/).

## El siguiente paso accionable (esta semana)

Elige **una** y hazla. No dos, no cero. Una.

- **Si tu cuadrante dijo SÍ**: ejecuta Graphify sobre tu repo más volátil esta semana. `graphify hook install`. Lee el `GRAPH_REPORT.md`. Registra dos surprising connections que no esperabas.
- **Si tu cuadrante dijo NO** pero tienes un `~/notes/` o vault personal heterogéneo: prueba un pase puntual sobre eso (≠ tu repo de código). Igual ahí sí compensa, igual no — datos antes que opinión.
- **Si tu cuadrante dijo "depende"**: aplica el lint determinístico a tu corpus actual antes de pagar Graphify. Muchos casos donde "necesito más estructura" se resuelven con un lint gratuito.
- **Si quieres profundizar**: instala MCP server local + conecta tu Cursor o Claude Desktop al grafo del starter. Experimenta con queries reales del día a día.

## Mensajes clave para el ponente

- **El cierre no es resumen**. Es **filtro**. La gente que se va sin un siguiente paso concreto no aplica nada. Insiste: en voz alta, "el de al lado tuyo te pregunta cuál es tu siguiente paso, y no puedes decir 'aún no sé'".
- **Recoger feedback corto** (anónimo si puede ser): "una cosa del taller que cambiarías", "una pregunta que te queda". Los talleres se ajustan con datos, no con sensación.
- **Mencionar el otro taller**: si los asistentes vienen del [taller-llm-wiki](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/proyectos/taller-llm-wiki.md), Graphify encaja como auditoría puntual sobre el wiki que montaron allí. Cierra el ciclo pedagógico.

## Cross-refs wiki

- `wiki/proyectos/taller-llm-wiki.md` — el taller hermano para los que vinieron del otro lado.
- `wiki/herramientas/graphify.md` — referencia técnica completa.
- `wiki/conceptos/instruction-budget.md` — el concepto que abrimos en el slide 2 y cerramos aquí.

## Cierre del cierre

"El veredicto razonado que tienes en el papel del Ejercicio 3 es **el output real del taller**. La herramienta es útil solo si entra a tu workflow con criterio. Gracias por venir, y nos vemos la semana que viene en `dev/Formaciones/`."
