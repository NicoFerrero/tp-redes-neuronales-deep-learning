# Memoria o patrones locales: RNN, LSTM y CNN 1D para clasificar movimiento

Trabajo práctico final de Redes Neuronales y Deep Learning
Maestría en Inteligencia Artificial, Universidad de Palermo
Alumno: Nicolás Ferrero · Profesor: Alan Dreszman

## Qué hace

Clasifica la actividad de una persona (caminar, subir escaleras, bajar escaleras, sentado, parado, acostado) a partir de las señales crudas del acelerómetro y el giroscopio de un celular. La entrada es una ventana de 2,56 segundos (128 pasos × 9 canales) y la salida es una de las 6 actividades.

Se comparan cuatro redes con alrededor de 25 mil parámetros cada una, más una regresión logística como referencia:

| Modelo | Accuracy en test | F1 macro |
|---|---|---|
| CNN 1D | 94,2% | 0,942 |
| CNN + LSTM | 92,9% | 0,930 |
| LSTM | 91,3% | 0,914 |
| RNN simple | 87,5% | 0,876 |
| Regresión logística | 58,3% | 0,554 |

El análisis completo está en `informe_nicolas_ferrero.pdf`.

## Contenido

- `tp_nicolas_ferrero.ipynb`: notebook con todo el trabajo, ya ejecutado.
- `informe_nicolas_ferrero.pdf`: informe.
- `resultados/`: figuras y `resultados.json` con los números de la corrida.
- - `docs/index.html`: página interactiva para estudiar el padding, el campo receptivo y el desvanecimiento del gradiente. Se puede usar en https://nicoferrero.github.io/tp-redes-neuronales-deep-learning/

## Cómo reproducirlo

1. Abrir `tp_nicolas_ferrero.ipynb` en Google Colab.
2. Elegir GPU en Entorno de ejecución > Cambiar tipo de entorno de ejecución > GPU T4.
3. Ejecutar todas las celdas (Entorno de ejecución > Ejecutar todas).

El notebook descarga el dataset solo desde el repositorio de UCI, así que no hace falta subir ningún archivo. Al terminar genera `resultados.zip` con todas las figuras y `resultados.json`. La corrida completa tarda unos 2 minutos con GPU.

También corre en una computadora local con Python 3.10 o superior y las librerías de abajo. Sin GPU funciona igual, pero más lento.

## Reproducibilidad

- Semilla fija: 0 (Python, NumPy y PyTorch, con cuDNN en modo determinístico).
- Partición por persona: 16 personas para entrenar, 5 para validación y las 9 del test oficial.
- Entrenamiento: Adam con tasa de aprendizaje 0,001, batch de 64, hasta 60 épocas con early stopping de paciencia 15, recorte de gradiente con norma máxima 1.

Versiones usadas en la corrida del informe:

| Librería | Versión |
|---|---|
| Python | 3.13.15 |
| PyTorch | 2.11.0 (CUDA 12.8) |
| NumPy | 2.1.3 |
| pandas | 2.2.3 |
| scikit-learn | 1.6.1 |

También usa matplotlib y seaborn para los gráficos.

## Dataset

UCI Human Activity Recognition Using Smartphones (Anguita y otros, 2013).
Link: https://archive.ics.uci.edu/dataset/240/human+activity+recognition+using+smartphones
Licencia: Creative Commons Attribution 4.0 International (CC BY 4.0).

30 personas de entre 19 y 48 años con un celular en la cintura, señales a 50 Hz cortadas en ventanas de 2,56 segundos con 50% de solapamiento.
