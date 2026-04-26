# Slide 11 — Más allá

## Tesis

Tienes el grafo, lo consultas, lo expones. Lo que queda es **automatizar el mantenimiento** y conectarlo con el resto de tu workflow. Cuatro palancas concretas, ordenadas por ratio impacto/coste.

## Las 4 palancas

### 1. Incremental con `--update` + cache SHA256

```bash
graphify --update <path>
```

Solo re-procesa ficheros modificados desde el último pase. El cache vive en `cache/` con hashes SHA256. Para code-only changes, **no llama al LLM** (capa 1 AST suficiente). Coste: fracción del primer pase.

Cuándo dispararlo: tras un día de trabajo, antes de cerrar sesión. O en CI tras merge a main.

### 2. Hooks git para rebuild automático

```bash
graphify hook install     # instala post-commit + post-checkout
graphify hook status
graphify hook uninstall
```

Tras cada `git commit`, el hook detecta qué code files cambiaron (vía `git diff HEAD~1`), re-extrae con AST y rebuild `graph.json`. **Doc/image changes** no trigger el hook (esas requieren capa 2 LLM, no automatizable sin coste). Para esas: `graphify --update` manual.

### 3. Vault Obsidian generado vs vault humano (firewall)

Si ya usas Obsidian para un vault personal:

- **Mal patrón**: `graphify --obsidian --obsidian-dir ~/vault/` — mete cientos de notas autogeneradas en tu vault humano.
- **Buen patrón**: `graphify --obsidian --obsidian-dir ~/vault/graphify/<proyecto>/` — namespace separado dentro del vault. Tu Graph view de Obsidian las verá pero no se mezclan con tus notas curadas.
- **Mejor patrón**: leer las generadas, importar manualmente las útiles a tu vault humano, borrar el resto. El generado es **draft**, no producto.

### 4. Save-result loop = memoria del agente

```bash
graphify save-result \
  --question "..." \
  --answer "..." \
  --type query \
  --nodes node1 node2
```

Cada respuesta valiosa que tu agente da consultando el grafo se guarda como Q&A en `graphify-out/memory/`. La próxima `--update` la incorpora como nodo nuevo, con aristas a los nodos citados. **El grafo aprende** de las consultas.

Es la conexión con el patrón [agente-aprendiz](https://github.com/CodigoSinSiesta/codigosinsiesta.github.io/blob/main/wiki/conceptos/agente-aprendiz.md): stateful agents con memoria operativa.

## Mensajes clave

- **El primer pase es caro, los siguientes son baratos**. Si solo haces el primer pase y nunca más, has tirado dinero. La rentabilidad real viene de hacer `--update` + queries durante semanas.
- **Hook git** es la palanca más infrautilizada. La gente lo mira como "complejidad extra" pero literalmente lo instalas con un comando y olvidarte. ROI inmediato.
- **El save-result loop es lo que diferencia un grafo de un RAG**. Tu RAG no aprende. El grafo persistente con save-result sí. Cada Q&A cerrada es un nodo nuevo. Es **acumulación**, no consumo.

## Cross-refs wiki

- `wiki/conceptos/agente-aprendiz.md` — el patrón de stateful agents.
- `wiki/herramientas/cleric.md` — caso real de AI SRE con memoria operativa, base teórica del save-result loop.
- `wiki/herramientas/graphify.md` — sección de comandos completos.

## Anécdotas posibles

- "Erin Ahmed (Cleric) cuenta que su AI SRE, tras 6 meses operando, había **escrito 200+ skills nuevas** a partir de incidentes resueltos. Cada incidente cerrado = una skill nueva = un nodo nuevo en el grafo del conocimiento del agente. Eso es save-result llevado a producto."

## Preguntas tipo

- *"¿Cuánto crece `graph.json` con el save-result loop?"* → Un Q&A cerrado son ~3-5 KB de overhead en `graph.json`. Ritmo: 50 Q&As/mes ≈ 250 KB/mes. Tu `graph.json` sigue siendo manejable durante años.
- *"¿El hook git me ralentiza el commit?"* → Code-only changes: 2-5 segundos de AST + rebuild del graph.json. Doc changes no trigger (lo dice el slide). Si te molesta, `--no-verify` puntual.

## Transición

"Tienes la herramienta, los outputs, la integración, el mantenimiento. Solo falta cerrar con qué te llevas."
