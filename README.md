# Proyecto final de NLP: traducción inglés-español

Evaluación comparativa de cuatro modelos de traducción automática neuronal
de inglés a español:

- **Modelo A:** BiLSTM con atención.
- **Modelo B:** Transformer compacto.
- **Modelo C:** Transformer base.
- **Modelo D:** Transformer preentrenado `Helsinki-NLP/opus-mt-en-es`.

El modelo D se evaluó antes y después de ajustarlo al corpus del proyecto.
La evaluación sin ajuste se incorporó como análisis adicional.

## Integrantes

- Ricardo Manuel Barbosa Flores — A00823815
- Kevin Bolaños Borja — A00838335

## Resultados principales

Los modelos se evaluaron sobre la misma partición de prueba, con
decodificación *greedy* y un límite de 128 tokens generados.

| Modelo | BLEU global | BLEU: 8–18 palabras | BLEU: más de 18 palabras |
|---|---:|---:|---:|
| A: BiLSTM con atención | 23.88 | 24.02 | 17.67 |
| B: Transformer compacto | 18.43 | 18.67 | 7.60 |
| C: Transformer base | 24.74 | 25.04 | 9.74 |
| D: preentrenado, sin ajuste fino | 55.57 | 55.54 | 56.90 |
| D: preentrenado, con ajuste fino | 57.40 | 57.30 | 60.59 |

El modelo preentrenado obtuvo la puntuación más alta. Después del ajuste
fino, su BLEU global aumentó de 55.57 a 57.40: una diferencia observada
de **1.83 puntos BLEU** en esta partición de prueba.

El grupo de más de 18 palabras contiene solo 31 oraciones, frente a
3,611 del grupo de 8 a 18 palabras. Por ello, los resultados del grupo
largo deben interpretarse con cautela.

### Pérdida y perplejidad en test

| Modelo | Pérdida en test | Perplejidad en test |
|---|---:|---:|
| A: BiLSTM con atención | 2.71 | 15.04 |
| B: Transformer compacto | 3.10 | 22.20 |
| C: Transformer base | 2.54 | 12.67 |
| D: preentrenado, sin ajuste fino | 0.68 | 1.97 |
| D: preentrenado, con ajuste fino | 0.56 | 1.74 |

El modelo D utiliza un tokenizador diferente al de A, B y C. Por ello,
su pérdida y su perplejidad por token **no son directamente comparables**
con las de los modelos entrenados desde cero. La comparación entre las
dos versiones de D sí utiliza el mismo tokenizador.

## Datos y protocolo

Se utilizó el corpus paralelo inglés-español de **Tatoeba/ManyThings**.
Se conservaron pares con entre 8 y 120 palabras en inglés y hasta
120 palabras en español. Después se mezclaron con la semilla 23 y se
dividieron en entrenamiento, validación y prueba.

| Partición | Pares |
|---|---:|
| Entrenamiento | 29,130 |
| Validación | 3,641 |
| Prueba | 3,642 |

- **Semilla de partición:** 23.
- **Test Audit ID:** `dbac04ac5648`.
- Los vocabularios de los modelos entrenados desde cero se construyeron
  únicamente con los datos de entrenamiento.
- Las épocas y la configuración del modelo ajustado se seleccionaron
  mediante la pérdida de validación.
- Para BLEU se aplicó la misma limpieza a predicciones y referencias:
  minúsculas y eliminación de acentos y signos de puntuación.
- La evaluación de D sin ajuste se realizó como análisis adicional,
  después de evaluar los sistemas principales.

## Contenido del notebook

El notebook incluye:

1. Descarga y preparación del corpus.
2. Partición y auditoría del conjunto de prueba.
3. Construcción de vocabularios y preparación de datos.
4. Entrenamiento de los modelos A, B y C desde cero.
5. Ajuste fino del modelo preentrenado D.
6. Curvas de aprendizaje y selección de checkpoints.
7. Evaluación en test mediante BLEU, pérdida y perplejidad.
8. Comparación del modelo D antes y después del ajuste fino.
9. Ejemplos cualitativos y mapas de atención para los modelos A y B.

Los checkpoints, historiales, predicciones y resultados se guardan en
Google Drive durante la ejecución; no se incluyen en este repositorio.

## Reproducción

1. Abrir `Proyecto_Final_NLP.ipynb` en Google Colab.
2. Activar una GPU; los experimentos reportados utilizaron una
   **Tesla T4**.
3. Ejecutar las celdas en orden y autorizar el montaje de Google Drive.
4. El notebook descargará el corpus si no encuentra `spa.txt` en la
   carpeta de trabajo.
5. Los checkpoints y resultados se guardarán en
   `MyDrive/Proyecto_NLP_NMT`.
6. Verificar que el identificador de auditoría de test sea
   `dbac04ac5648`.

El entrenamiento completo requiere tiempo de GPU y espacio disponible
en Drive. Si existen checkpoints de una ejecución anterior en la carpeta
de trabajo, el notebook puede reutilizarlos para continuar el proceso.

La descarga original del corpus puede cambiar con el tiempo. Para
reproducir exactamente la partición y el Audit ID se necesita la misma
copia de `spa.txt` utilizada en el experimento. El Audit ID identifica
la secuencia de entradas en inglés del conjunto de prueba; por sí solo
no verifica todas las características del corpus ni del entorno.

## Limitaciones

- No se verificó si las frases de test aparecen en los datos de
  preentrenamiento de `opus-mt-en-es`.
- La comparación antes y después del ajuste corresponde a una sola
  partición de prueba y a las ejecuciones reportadas.
- El grupo de oraciones largas contiene únicamente 31 ejemplos.
- La evaluación utiliza una referencia por oración y decodificación
  *greedy*; otros procedimientos podrían producir resultados distintos.
- Los modelos difieren en arquitectura, tokenización y entrenamiento,
  por lo que las comparaciones no aíslan individualmente cada factor.
- Los tiempos de ejecución y latencia pueden variar según el hardware
  y las condiciones de la sesión.
