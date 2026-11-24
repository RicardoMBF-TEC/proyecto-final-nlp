# Proyecto final de NLP: traducción inglés-español

Comparación de cuatro modelos de traducción automática neuronal:
BiLSTM con atención, Transformer compacto, Transformer base y
Transformer preentrenado con ajuste fino.

## Resultados principales

| Modelo | BLEU global en test |
|---|---:|
| BiLSTM con atención | 23.88 |
| Transformer compacto | 18.43 |
| Transformer base | 24.74 |
| Modelo preentrenado ajustado | 57.40 |

El modelo preentrenado obtuvo el mayor BLEU. No se verificó el
posible solapamiento entre sus datos de preentrenamiento y el test.

## Datos y auditoría

- Corpus: Tatoeba/ManyThings, inglés-español.
- Entrenamiento: 29,130 pares.
- Validación: 3,641 pares.
- Prueba: 3,642 pares.
- Semilla de partición: 23.
- Test Audit ID: `dbac04ac5648`.

## Reproducción

1. Abrir el notebook `.ipynb` en Google Colab.
2. Activar una GPU si está disponible.
3. Ejecutar las celdas en orden y autorizar el montaje de Google Drive.
4. El notebook descarga el corpus y guarda checkpoints y resultados
   en la carpeta `Proyecto_NLP_NMT` de Drive.
5. El entrenamiento completo requiere tiempo de GPU y espacio en Drive.

La descarga original del corpus puede cambiar con el tiempo. Para
reproducir exactamente la auditoría se necesita la misma copia de
`spa.txt` usada en el experimento.
