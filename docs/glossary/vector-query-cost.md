# vector-query-cost

Costo economico de una consulta que usa retrieval sobre base vectorial.

No se limita al embedding de la consulta. Debe considerar por separado:

- el embedding del query;
- el contexto recuperado;
- los tokens que ese contexto vuelve a inyectar al modelo generativo final.

Este concepto existe para no mezclar ingestion inicial con costo recurrente de uso RAG.
