# token-accounting

Practica de descomponer el costo de una interaccion AI segun los tokens que realmente viajan por cada etapa.

En `cost-console` significa distinguir al menos:

- tokens de `input`;
- tokens de `output`;
- tokens de contexto reenviado por historial o retrieval.

No es solo mostrar un costo final. Su objetivo es explicar de donde sale ese costo y como cambia cuando crece el historial, el contexto o la respuesta esperada.
