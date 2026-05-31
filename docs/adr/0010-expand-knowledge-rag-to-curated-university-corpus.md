# ADR 0010: Expand Knowledge RAG to a Curated University Corpus from Google Drive and GitHub

- Date: 2026-05-31
- Status: Accepted

## Context

ADR 0009 definio `knowledge-rag` como una app RAG externa al ecosistema central del portafolio y fijo una carpeta propia de Google Drive como corpus inicial demostrable.

Esa decision resolvio correctamente la frontera del proyecto, pero dejo acotada de mas la definicion del corpus. El caso de uso real ahora esta mas claro: construir una memoria consultable del paso por la universidad usando tanto documentos de una carpeta curada de Google Drive como repositorios publicos de GitHub relacionados con materias, guias y resoluciones.

El problema ya no es decidir si `knowledge-rag` vive o no dentro del ecosistema central; eso ya quedo cerrado. El problema ahora es definir que fuentes externas forman parte del corpus inicial y bajo que restricciones para que la app siga siendo demostrable, explicable y controlable.

## Decision Drivers

- Mantener a `knowledge-rag` como proyecto visible del punto `2` sin reabsorberlo en el ecosistema central.
- Convertir el corpus en una representacion mas fiel del recorrido universitario real.
- Permitir respuestas con citas tanto sobre material teorico como sobre resoluciones y codigo fuente.
- Evitar que el corpus se convierta en una biblioteca indiferenciada o en un volcado completo de archivos sin curacion.
- Preservar control operativo simple en la primera fase mediante sincronizacion manual por lote.

## Decision

`knowledge-rag` sigue siendo una app RAG standalone externa al ecosistema central, pero su corpus inicial deja de limitarse a Google Drive y pasa a definirse como un corpus universitario curado compuesto por:

- una o mas carpetas concretas de Google Drive;
- uno o mas repositorios publicos de GitHub claramente ligados a materias, guias o resoluciones.

La primera forma del proyecto debe:

- sincronizar metadata y contenido extraido de ambas fuentes;
- organizar el conocimiento principalmente por `materia` y `semestre`;
- ejecutar chunking, embeddings, indexacion y retrieval sobre documentos y codigo fuente;
- responder preguntas con citas verificables sobre ese corpus;
- operar con sincronizacion manual por lote, no continua.

Adicionalmente:

- Google Drive y GitHub siguen siendo source of truth de los archivos y repositorios;
- `knowledge-rag` guarda derivados semanticos y referencias de sincronizacion, no la propiedad maestra del contenido;
- la primera version debe aceptar corpus curado, no indexacion indiscriminada de todo el Drive o de cualquier repo disponible;
- cualquier integracion futura con `other-gpt`, `mcp-server` o `ai-gateway` sigue siendo opcional y posterior.

## Consequences

### Positive

- El punto `2` gana un caso de uso mas fuerte y mas personal que una carpeta documental aislada.
- El corpus puede combinar teoria, apuntes, guias, presentaciones y resoluciones en codigo.
- Las respuestas con citas pueden apoyarse tanto en documentos como en archivos fuente publicos.
- La organizacion por `materia` y `semestre` reduce ruido y hace mas explicable el retrieval.

### Negative

- Aumenta la heterogeneidad del corpus y con ello la complejidad de extraccion y chunking.
- Los repositorios completos pueden introducir ruido, duplicados o codigo irrelevante si no se filtran bien.
- La historia de permisos ya no depende solo del alcance de Google Drive, sino tambien de disponibilidad publica y estructura de GitHub.

## Implementation Notes

- Las fuentes iniciales del corpus se registran como referencias curadas a carpetas de Google Drive y repositorios publicos de GitHub.
- La app debe persistir al menos referencias de origen, texto extraido, chunks, embeddings, citas, metadata academica y estado de sincronizacion.
- La metadata minima esperada por documento o archivo indexado incluye `materia`, `semestre`, `origen` y `tipo_fuente`.
- La indexacion debe poder citar paginas, slides, segmentos textuales o rangos de lineas segun el tipo de contenido.
- La operacion inicial prioriza sync manual por lote y observabilidad de errores por archivo o fuente.

## Related Decisions

- Este ADR supersede a ADR 0009 en lo relativo al alcance del corpus inicial de `knowledge-rag`.
- ADR 0004 separa persistencia de chat, assets, RAG y MCP.
- ADR 0005 mantiene a `ai-gateway` como control-plane multi-provider y evita convertirlo en owner de retrieval.
- ADR 0007 mantiene visible el costo de ingestion y retrieval aunque `knowledge-rag` viva fuera del ecosistema central.
