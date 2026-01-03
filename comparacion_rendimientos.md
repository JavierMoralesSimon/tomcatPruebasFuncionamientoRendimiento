# Rendimiento de una aplicación

## Instalación de ApacheBench
Instalamos la herramienta para realizar pruebas de funcionamiento y rendimiento llamada ApacheBench mediante el comando `sudo apt install apache2-utils`.
![Instalación de ApacheBench](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/1.1.png)

## Ejecución antes de los cambios
Ejecutamos la herramienta sobre nuestra aplicación para ver cual es su estado actual en cuanto a funcionamiento y rendimiento mediante el comando `ab -n 1000 -c 50 http://localhost:8080/helloapp2/helloapp/HelloWorld.jsp`.
![Ejecución de ApacheBench antes de hacer cambios](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/1.2.1.png)
![Ejecución de ApacheBench antes de hacer cambios](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/1.2.2.png)

## Modificación del archivo "server.xml"
  1. Accedemos a la ruta en la que se encuentra el archivo y lo abrimos.
    ![Acceso al archivo "server.xml"](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/2.1.png)
  2. Buscamos el texto que se muestra en la siguiente captura.
    ![Texto de "server.xml" antes de ser modificado](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/2.2.png)
  3. Realizamos alguna modificación que en mi caso es poner "connectionTimeout" en 10000 y "maxThreads" en 300.
    ![Texto de "server.xml" después de ser modificado](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/2.3.png)
  4. Reiniciamos Tomcat para aplicar los cambios.
    ![Reinicio de Tomcat](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/2.4.png)

## Ejecución después de los cambios
Ejecutamos de nuevo la herramienta sobre nuestra aplicación para ver cual es su estado tras los cambios realizados.
![Ejecución de ApacheBench después de hacer cambios](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/3.1.png)
![Ejecución de ApacheBench después de hacer cambios](https://github.com/JavierMoralesSimon/tomcatPruebasFuncionamientoRendimiento/blob/main/Capturas/3.2.png)

## Comparación de los rendimientos antes y después
Rendimiento general:
| Métrica                 | ANTES      | DESPUÉS    | Cambio        |
| ----------------------- | ---------- | ---------- | ------------- |
| Requests/sec            | 470.93     | 330.79     | –29.8%        |
| Time per request (mean) | 106 ms     | 151 ms     | +42%          |
| Tiempo total            | 2.12 s     | 3.02 s     | Más lento     |
| Failed requests         | 0          | 0          | Bien          |

Latencias:
| Percentil  | ANTES (ms) | DESPUÉS (ms) | Cambio         |
| ---------- | ---------- | ------------ | -------------- |
| 50%        | 95         | 109          | Incremento     |
| 75%        | 125        | 144          | Incremento     |
| 90%        | 170        | 183          | Incremento     |
| 95%        | 203        | 210          | Incremento     |
| 99%        | 254        | 256          | Igual          |
| 100% (máx) | 286        | 676          | Incremento     |

Tiempos de conexión vs procesamiento:
| Métrica         | ANTES  | DESPUÉS    |
| --------------- | ------ | ---------- |
| Mean connect    | 1 ms   | 1 ms       |
| Max connect     | 33 ms  | 10 ms      |
| Mean processing | 103 ms | 116 ms     |
| Max processing  | 286 ms | 675 ms     |

Observaciones:
  * El tuning aplicado redujo el throughput y aumentó la latencia.
  * El peor request pasó de 286 ms a 676 ms, lo cual es más del doble.
  * Alto incremento del procesamiento interno.
