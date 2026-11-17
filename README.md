# Astheia AI: Modelo de Detección de Cuchillos

Este paquete contiene el código y los resultados del entrenamiento del modelo usado en Astheia AI. La aplicación está en un paquete aparte.

## Contenido

- `Astheia_AI_Modelo.ipynb`: cuaderno de Google Colab con todo el proceso: preparación de los datos, partición, entrenamiento, evaluación y prueba con videos externos. Las salidas que se ven en el cuaderno son las de las ejecuciones originales.
- `congelar_particion.py`: script que genera la partición por grupos.
- `yolo_v3/`: manifiesto de la partición final (`particion_congelada.json`) y su `data.yaml`. Se incluyen los nombres de las imágenes, no las imágenes.
- `corridas/`: resultados de cada entrenamiento (`results.csv`, `args.yaml`, gráficas y pesos `best.pt`).
- `videos_externos.csv`: alertas generadas sobre los videos del Shoplifting Video Dataset.

## Corridas Incluidas

| Corrida | Modelo | Uso | mAP50 validación | mAP50 prueba |
|---|---|---|---|---|
| `final_v8s` | YOLOv8s | Modelo final de la aplicación (45 épocas, mejor época 35) | 0,756 | 0,963 |
| `v5su_b` | YOLOv5su | Modelo de comparación (50 épocas, mejor época 47) | 0,730 | 0,952 |
| `baseline_v1_gpu` | YOLOv8s | Primer modelo, entrenado con la partición inicial que tenía fuga de datos. Se conserva como evidencia de esa lección aprendida | No comparable | No comparable |

Las corridas de prueba, interrumpidas o repetidas no se incluyen.

## Datos

Las imágenes provienen del subconjunto SOHAS Detection del repositorio OD-WeaponDetection (Pérez-Hernández et al., 2020), disponible en Dataset Ninja. Se conservó solo la clase **knife**, se descartaron las imágenes con pistolas y las demás clases se usaron como imágenes negativas.

La partición final tiene 3.045 imágenes de entrenamiento, 652 de validación y 652 de prueba, de las cuales 1.560, 377 y 340 tienen cuchillo. Las imágenes casi idénticas (aHash, distancia de Hamming ≤ 3) y los fotogramas de un mismo video se asignaron siempre a la misma partición.

Los videos externos provienen del Shoplifting Video Dataset de Kaggle (Kipshidze).

Ni las imágenes ni los videos se incluyen en este paquete por su tamaño y porque pertenecen a sus autores. Con el manifiesto y el script se puede reconstruir la partición a partir de la fuente original.

## Entorno

Google Colab con GPU Tesla T4, Python 3.13 y Ultralytics 8.4. Entrenamiento con imágenes de 640 px, lotes de 16, máximo 50 épocas y parada temprana con paciencia de 10.

Más detalles en la monografía **Astheia AI: Modelo para la Detección de Armas Blancas en Video con Verificación Humana**.
