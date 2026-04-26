# Slide 6 — El cuadrante de decisión ⭐

## Tesis

Graphify no es la única forma de dar contexto a un agente. Su sweet spot es estrecho y específico. **Conocer las 3 alternativas** y dónde gana cada una es lo que separa al usuario crítico del promotor entusiasta.

## El cuadrante (slide visual: tabla 4×4)

| Eje | GitNexus | Graphify | Vector RAG | Agentic search puro |
|-----|----------|----------|------------|---------------------|
| **Scope** | Solo código | Código + docs + papers + imágenes + vídeo | Texto plano (cualquiera) | Filesystem (cualquiera) |
| **Determinismo** | 100% AST | AST + LLM | Embedding-based | Heurístico |
| **Coste** | Gratis | ~$0.30-0.80 por corpus medio | Embedding cost + storage | Tokens del agente |
| **Privacidad** | Código nunca sale | LLM ve docs | Embeddings van a la API | Agente lee localmente |
| **Persistencia** | Sí (KuzuDB) | Sí (graph.json) | Sí (vector DB) | No (regenera por sesión) |
| **MCP nativo** | ✓ | ✓ vía `--mcp` | Solo con wrapper | n/a |

## Mensajes clave

- **GitNexus** ([repo](https://github.com/abhigyanpatwari/GitNexus)): si tu corpus es **solo código** y privacidad importa, gana sin discusión. AST puro, KuzuDB embebido, MCP nativo, gratis. Es el equivalente "FOSS" a la capa 1 de Graphify, sin las capas 2-4.
- **Graphify**: gana cuando tu corpus es **heterogéneo** (Karpathy `/raw` típico — código + papers + tweets + screenshots). La capa 2 (semántica) es lo que cobra y lo que aporta. Si tu corpus no la necesita, no la pagues.
- **Vector RAG**: gana en **corpus enorme y plano** donde lo único que importa es "encuéntrame algo similar". Falla en preguntas que cruzan documentos (X depende de Y disperso entre 3 docs).
- **Agentic search puro** (Claude Code con `rg`+`cat`): gana en **corpus pequeño** (cabe en context budget al leer-on-demand) o **consultas únicas** (no se amortiza el coste de indexar). Determinístico. Gratis salvo los tokens del agente.

## Reglas prácticas para elegir

1. **¿Cabe tu corpus relevante en <30k tokens?** → Agentic search puro. No indexes nada.
2. **¿Solo código y privacidad alta?** → GitNexus.
3. **¿Heterogéneo y queries cross-document frecuentes?** → Graphify.
4. **¿Corpus enorme y queries de "encuéntrame parecido"?** → Vector RAG.
5. **¿Combinaciones?** → Sí. Vector para "narrow", grafo para "navegar". Empresas grandes hacen híbridos.

## Cross-refs wiki

- `wiki/herramientas/gitnexus.md` — el competidor para repos puros.
- `wiki/herramientas/graphify.md` — la sección *Posicionamiento* tiene esta misma comparativa con más detalle.
- `wiki/conceptos/agentic-search-vs-rag.md` — el eval de Braintrust con datos reales: agentic 68% vs vector 60% en bug fixing, 4× más barato.

## Anécdotas posibles

- Jessica Wang (Braintrust) construyó un eval reproducible: en SWE-Bench Django, agentic search bate a vector RAG por 8 puntos y cuesta 4× menos. Esto es **agentic search puro vs vector**, sin grafo. El grafo es una tercera vía complementaria, no un reemplazo.
- En el experimento sobre el wiki personal: **descartamos Graphify para uso regular** porque el corpus (~85 docs markdown, 57k palabras) cabe en agentic search puro y el MOC manual ya hace lo que el clustering automático ofrecería. El cuadrante se aplica honestamente.

## Preguntas tipo

- *"¿Y si combino Graphify con Vector RAG?"* → Es válido. Pero te toca operar dos pipelines y tu agente tiene que saber cuándo usar cuál. Coste-beneficio: razonable solo si tu corpus es claramente bicapa (uno enorme y plano + uno chico y conectado).
- *"¿GitNexus puede leer mis docs?"* → No. Es solo AST sobre código. Si tu repo tiene docs ricos en `docs/`, GitNexus los ignora y Graphify los aprovecha.

## Transición

"El cuadrante te da la teoría. Ahora vamos a ver el caso práctico — el que hicimos sobre este propio wiki — para que veas cómo se aplica con datos honestos."
