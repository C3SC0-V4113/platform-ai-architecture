# knowledge-rag

App RAG especializada en convertir contenido externo en conocimiento recuperable.

En la definicion actual del portafolio, `knowledge-rag` vive fuera del ecosistema central y usa una carpeta de Google Drive como corpus inicial. Guarda derivados semanticos de esos documentos: texto extraido, chunks, embeddings, citas y referencias de sincronizacion. No debe ser tratado como el almacenamiento maestro de adjuntos del ecosistema.

El hecho de que `knowledge-rag` produzca chunks y embeddings no lo convierte en owner de la simulacion economica de esos flujos. Esa responsabilidad visible dentro del portafolio vive en `cost-console`.

Aunque es una habilidad importante, su integracion con `other-gpt`, `mcp-server` o `ai-gateway` no es requisito temprano; primero debe demostrar valor como app RAG standalone.
