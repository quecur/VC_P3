# VC_P3


# Introducción

En esta práctica se ha tenido que realizar la elaboración dos modelos limitados por el uso de Visión por Computador, es decir, para la extracción de características y segmentación de la imagen tan solo se ha ultilizado la libreria de OpenCV. Esto ha supuesto un verdadero quebradero de cabeza en ambos casos, por un lado en el caso del modelo de detección de monedas la extracción de características ha supuesto un verdadero reto y en el caso de la detección de microplásticos la dificultad ha estado en la segmentación de la imagen sobre todo en el caso de los fragmentos. Es por ello que los modelos tienen unan presición bastante límitado pero en la medida de lo posible he tratado que sean generalizables para detecciones en circunstacias similares.

# Tarea 1

TAREA: Captura una o varias imágenes con monedas no solapadas. Tras visualizar la imagen, identifica de forma interactiva (por ejemplo haciendo clic en la imagen) una moneda de un valor determinado en la imagen (por ejemplo de 1€). Tras ello, la tarea se resuelve mostrando por pantalla el número de monedas y la cantidad de dinero presentes en la imagen. No hay restricciones sobre utilizar medidas geométricas o de color. ¿Qué problemas han observado?
Nota: Para establecer la correspondencia entre píxeles y milímetros, comentar que la moneda de un euro tiene un diámetro de 23.25 mm. la de 50 céntimos de 24.35, la de 20 céntimos de 22.25, etc.
Extras: Considerar que la imagen pueda contener objetos que no son monedas y/o haya solape entre las monedas. Demo en vivo.


He enfocado la elaboración de esta tarea de tres formas diferentes, finalmente opte por la menos mala, no obstante no estoy nada satisfecho con el resultado. 

En primer lugar trate de generalizar la detección fijandome en el interior de la moneda y de esta forma esta no estar limitado por la escala de la foto, para ello intente extraer como característica la detección de bordes en el interior de la imagen para así tratar de asignar según la proporción de bordes un valor a la moneda, sin embargo, esto fue un fracaso absoluto, este enfoque se ve muy limitado por la segmentación del interior de la moneda el cual se hace muy dificil por obstaculos como el ruido y la iluminación en la imagen.

En segundo lugar para no tener que recurrir al tamaño por el hecho de que este se ve muy afectado por el angulo y la escala de la imagen, intente extraer como característica principal la detección del color, de esta forma si conseguía detectar una moneda de 1 euro o 2 euros en la imagen gracias al color me sería mas sencillo detectar el resto de monedas en la imagen, para ello trate de calcular una especie de proporción de plateado en la imagen. Mediante este enfoque tampoco obtuve ningún resultado concluyente puesto que al igual que en el caso anterior, la detección del color también se vio bastante afectada por el ruido y la iluminación de la imagen.

Finalmente termine por resignarme, para el tercer enfoque me limite a usar el tamaño como característica principal, esto no es muy buena idea, como ya mencione esta característica se ve muy limitada por la escala y el angulo de la imagen, otro de los problemas que observe al recurrir a esta característica es que al ser la diferencia de tan solo milimetros entre unas monedas y otras la segmentación ha de ser muy precisa y esto resulta bastante complicado pues en cada foto el tamaño del diametro que se dibuja en la moneda varía independiente de que se trate de la misma escala. Trate de dibujar las circunferencias con aproximación de contornos y con la transformada de Hough optando por esta última, pero en ninguno de los dos casos obtuve buenos resultados. Para el modelo final la detección se basa en encontrar una moneda de dos euros fijandose en el area mas grande de los circulos detectados y a partir de ahí obtener el resto de monedas que se observan en la imagen comparando su area con la de la moneda de dos euros y respecto a esta determinar de que moneda se trata. Tome como referencia el tamaño real de las monedas pero en todo caso el modelo no funciona correctamente.


![image](https://github.com/user-attachments/assets/a6bb6054-6f09-4507-baf9-548fe45948bd)

![image](https://github.com/user-attachments/assets/6b6fb2d0-d88f-455f-b66e-b6392da57db4)


Conclusión: La extracción de características de una imagen mediante visión por computador se ve significativamente afectada por diversas variables que pueden comprometer su calidad. Para mejorar este modelo, sería necesario enfocarse en optimizar la segmentación de las características mencionadas, o bien encontrar alguna otra propiedad que resultase útil.

# Tarea 2

TAREA: Las tres imágenes cargadas en la celda inicial, han sido extraidas de las imágenes de mayor tamaño presentes en la carpeta. La tarea consiste en extraer características (geométricas y/o visuales) e identificar patrones que permitan distinguir las partículas de cada una de las tres clases, evaluando los aciertos y fallos con las imágenes completas considerando las métricas mostradas y la matriz de confusión. La matriz de confusión, muestra para cada clase el número de muestras que se clasifican correctamente de dicha clase, y el número de muestras que se clasifican incorrectamente por cada una de las otras dos clases.


En primer lugar cargamos las imagenes, recortamos previamente la imagen de los pellets y la de los fragmentos para eliminar el ruido. Puestos a recortarla en el codigo de forma arbitraria la recorte directamente fuera y genere una nueva imagen.


![image](https://github.com/user-attachments/assets/371a2f50-3def-456f-908e-f6a21f03fe48)


Segmentamos las imagenes extraidas de los conjuntos totales para optener compacticidad de los pellets y fragmentos además del color medio de los trozos de alquitran


![image](https://github.com/user-attachments/assets/f2ac5fe4-55b7-4b55-998b-71f11970e1eb)

![image](https://github.com/user-attachments/assets/523a4731-e39c-4354-ab68-889939e6187f)

![image](https://github.com/user-attachments/assets/c06722d4-681e-4397-90ff-b3f68d768cc2)

![image](https://github.com/user-attachments/assets/09eda7a5-dab3-4a16-9d7a-80adc8f3dea3)


Finalmente segmentamos las imagenes que contienen todos los pellets, microplásticos y trozos de alquitran y clasificamos una a uno los contornos obtenidos.


![image](https://github.com/user-attachments/assets/98802cef-7809-40b8-bff2-2d44afb65ec8)

![image](https://github.com/user-attachments/assets/cfcb6c92-8611-4886-ba93-f6b7305c309b)


Obtenemos la matriz de confusión. (Mismo código que en el ejemplo)


![image](https://github.com/user-attachments/assets/3126f60c-8803-473d-9327-19826e830e48)

![image](https://github.com/user-attachments/assets/8e1c62e6-760a-4d7a-89a6-32ee5e5bdcea)


Conclusión: la verdad que los resultados del modelo en no son malos del todo, posee una precisión bastante decente puesto que las heuristicas usadas para clasificar los contornos son bastante reveladoras, sin embargo, no se debe ignorar el hecho de que la detección de fragmentos ha sido insuficiente e imprecisa, habría que mejorar la segmentación.







