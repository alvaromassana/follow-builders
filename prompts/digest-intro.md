# Prompt de Introducción al Digest

Estás ensamblando el digest final a partir de los resúmenes individuales de cada fuente.

## Formato

Empieza con esta cabecera (sustituye [Fecha] por la fecha de hoy):

Digest de Builders de IA — [Fecha]

Luego organiza el contenido en este orden:

1. Sección X / TWITTER — lista cada builder con sus nuevos posts
2. Sección BLOGS OFICIALES — lista cada artículo de los blogs oficiales de empresas de IA (OpenAI, Anthropic, etc.)
3. Sección PODCASTS — lista cada podcast con los nuevos episodios

## Reglas

- Solo incluye fuentes que tengan contenido nuevo
- Omite cualquier fuente que no tenga nada nuevo
- Bajo cada fuente, pega el resumen individual que has generado

### Enlaces de podcasts
- Después de cada resumen de podcast, incluye la URL específica del vídeo desde el campo `url` del JSON
  (p. ej. https://youtube.com/watch?v=Iu4gEnZFQz8)
- NUNCA enlaces a la página del canal. Siempre al vídeo concreto.
- Incluye el título exacto del episodio desde el campo `title` del JSON en el encabezado

### Formato de autores de tweets
- Usa el nombre completo del autor y su rol/empresa, no solo el apellido
  (p. ej. "Aaron Levie, CEO de Box", no "Levie")
- NUNCA escribas los handles de Twitter con @ en el digest. En clientes de email, @handle puede convertirse en un enlace incorrecto. Escribe los handles sin @ (p. ej. "Aaron Levie (levie en X)" o simplemente usa su nombre completo)
- Incluye el enlace directo a cada tweet desde el campo `url` del JSON

### Formato de posts de blog
- Usa el nombre del blog como encabezado de sección (p. ej. "Anthropic Engineering", "OpenAI News", "Claude Blog")
- Bajo cada blog, lista cada post nuevo con su título y resumen
- Incluye el autor si está disponible
- Incluye el enlace directo al artículo original

### Enlaces obligatorios
- CADA pieza de contenido DEBE tener un enlace a la fuente original
- Posts de blog: la URL directa del artículo (p. ej. https://www.anthropic.com/engineering/...)
- Podcasts: la URL del vídeo de YouTube (p. ej. https://youtube.com/watch?v=xxx)
- Tweets: la URL directa del tweet (p. ej. https://x.com/levie/status/xxx)
- Si no tienes enlace para algo, NO lo incluyas en el digest.
  Sin enlace = no es real = no se incluye.

### Nada de inventar
- Solo incluye contenido que venga del JSON del feed (blogs, podcasts y tweets)
- NUNCA te inventes citas, opiniones ni contenido que creas que alguien pudo haber dicho
- NUNCA especules sobre el silencio de alguien ni sobre lo que pudiera estar trabajando
- Si no tienes nada real sobre un builder, omítelo entero

### General
- Al final del todo, añade una línea: "Generado con la skill Follow Builders (fork Websalia): https://github.com/alvaromassana/follow-builders"
- Mantén el formato limpio y escaneable — esto se leerá en una pantalla de móvil
- **Idioma del digest: español.** Los nombres propios de personas, productos y empresas y los términos técnicos (LLM, agent, prompt, fine-tuning, transformer, RAG, API, token, GPU) se dejan en inglés. Todo lo demás en español natural.

## Triage: sección "Candidatos detectados" al final

Antes del footer, añade una sección `## Candidatos detectados` con los items que menciones tools/repos/features concretos evaluables. No analices aquí — solo lista. El scout real lo ejecuta Álvaro manualmente.

### Reglas de clasificación

Clasifica cada item del feed en una de tres categorías (usa tu criterio leyendo el contenido):

- **POINTABLE** — menciona una herramienta, repo, CLI, MCP server, skill, API o feature nueva con URL o nombre concreto. Candidato a scout.
- **PATTERN** — describe una técnica replicable sin link a producto (ej: "uso X para Y haciendo Z"). Documentable en `vault/system/techniques/` sin scout.
- **INFORMATIVE** — opinión/análisis de mercado/hype/humor/observación lateral sin acción derivable. Skip.

### Pre-filtros obligatorios

Antes de listar un item como candidato:

1. **Dedup contra scouts previos**: el contexto te pasa una lista `already_scouted_slugs`. Si el target coincide con un slug ya evaluado → skip con nota breve "dedup".
2. **Lista negra recurrente**: el contexto te pasa `non_candidates` (gente cuyo contenido suele ser INFORMATIVE). Si el post viene de esa lista y no es una excepción clara → skip.
3. **Empresas proveedor conocidas**: skip OpenAI, Anthropic, Google, Microsoft, Meta, Amazon, Vercel, Cloudflare, Railway, Replit, Cursor, GitHub, GitLab — son infraestructura base, no targets de scout.

### Formato de la sección

```markdown
## Candidatos detectados

> Para ejecutar scouts, hazlo desde tu sesión **Opus** (no escatimar calidad en investigación).

### 🔗 <nombre del target> (POINTABLE)

Breve de una frase. Mencionado por <Builder>. Por qué es relevante para el sistema: <1 frase>.

`/scout <url o target>`

### 🔗 <nombre del target> (POINTABLE)

...

### 📐 <nombre del patrón> (PATTERN)

Breve de una frase. Documentable sin scout. Mencionado por <Builder>.
```

Si no hay candidatos en un día concreto, añade: "Hoy sin candidatos. Todo el contenido fue INFORMATIVE."

No incluyas INFORMATIVE items en esta sección. No incluyas tu razonamiento, solo la lista final.

### Persistencia

Antes de enviar el email, escribe también la sección de candidatos como JSON estructurado en `/home/alvaro/vault/shared/tasks/ai-digest-candidates/<YYYY-MM-DD>.json` para trazabilidad histórica:

```json
{
  "date": "YYYY-MM-DD",
  "candidates": [
    {"type": "POINTABLE", "target": "...", "url": "...", "source_builder": "...", "source_post_url": "...", "relevance": "..."}
  ],
  "skipped_dedup": ["slug1", "slug2"],
  "skipped_non_candidates": ["builder1", "builder2"]
}
```

