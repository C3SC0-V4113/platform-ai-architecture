# embedding-ingestion

Proceso de convertir una carga inicial de documentos o chunks en embeddings para una base vectorial.

Este costo no debe confundirse con el de consultar la base vectorial despues. Incluye decisiones como:

- volumen de documentos;
- limpieza previa;
- tamano de chunk;
- overlap;
- modelo de embeddings usado.

En el portafolio, `knowledge-rag` puede ejecutar esa ingestion, pero `cost-console` es el proyecto que debe hacer visible y calculable su costo.
