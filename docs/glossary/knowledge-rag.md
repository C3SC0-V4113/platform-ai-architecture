# knowledge-rag

App RAG especializada en convertir contenido externo en conocimiento recuperable.

En la definicion actual del portafolio, `knowledge-rag` vive fuera del ecosistema central y usa un corpus universitario curado compuesto por carpetas de Google Drive y repositorios publicos de GitHub. Guarda derivados semanticos de esos documentos y archivos fuente: texto extraido, chunks, embeddings, citas y referencias de sincronizacion. No debe ser tratado como el almacenamiento maestro de adjuntos del ecosistema.

El hecho de que `knowledge-rag` produzca chunks y embeddings no lo convierte en owner de la simulacion economica de esos flujos. Esa responsabilidad visible dentro del portafolio vive en `cost-console`.

Aunque es una habilidad importante, su integracion con `other-gpt`, `mcp-server` o `ai-gateway` no es requisito temprano; primero debe demostrar valor como app RAG standalone con sincronizacion manual por lote y respuestas con citas verificables.
