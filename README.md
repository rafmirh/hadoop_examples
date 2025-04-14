# hadoop_examples
Hadoop examples for Herramientas Avanzadas para el Manejo de Grandes Volumenes de datos course teaching at UNRC  

## Prerequisitos
Tener instalado Docker Desktop  

## Referencias
Tomamos la estructura de **docker-hadoop** de [https://github.com/big-data-europe/docker-hadoop](https://github.com/big-data-europe/docker-hadoop)  

## Descargamos el material de trabajo:
Del enlace anterior, descargamos el proyecto como .zip y posteriormente lo descomprimimos en una ruta de nuestra elección.  

![image](https://github.com/user-attachments/assets/8c01086c-ca54-4c2f-82cb-db2b8a58181d)  


Abrimos una terminal en la caroeta de nuestro proyecto. Para esto, damos click derecho en un área libre del explorador de archivos y seleccionamos la opción "abrir en terminal":  

![image](https://github.com/user-attachments/assets/fdfde2b7-e224-4969-9e80-912df41e3851)  


En la terminal ejecutamos el comando:
`docker compose up -d`  

![image](https://github.com/user-attachments/assets/e4ccb3f5-717a-4d02-8585-b2df4d1eed03)  


## Entrar al nodo maestro `namenode`
Ejecutar `docker exec -t namenode bash`  

![image](https://github.com/user-attachments/assets/06a597d2-88aa-48e2-b5fe-ea971f10f8ed)  

## Crear estructura `/user/root/`
En la terminal escribimos el comando `hdfs dfs -mkdir -p /user/root/`  

![image](https://github.com/user-attachments/assets/9e165751-f834-462a-8ff2-9ccf28c260d6)  

## Script para conteo de palabras mapreduce
Utilizamos un ejecutable java con el algoritmo para realizar el MapReduce. En este caso, descargaremos el algoritmo ejemplo llamado [hadoop-mapreduce-examples-2.7.1-sources.jar](https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/) el cual guardaremos en la misma carpeta de nuestro proyecto `hadoop_examples`  



## Ejemplo de MapReduce

### Contar palabras
Para este ejemplo, se contarán las palabras del libro  `Satoshi Yagisawa - Mis días en la librería Morisaki`  el cual añadimos a la carpeta de nuestro proyecto como archivo simple de texto *.txt

![image](https://github.com/user-attachments/assets/f49799a7-5b43-4970-8ec4-aabd18c10c2d)

### Contar palabras  
En la terminal, regresamos a nuestra carpeta del proyecto utilizando el comando `exit`  

![image](https://github.com/user-attachments/assets/aa0b8d99-86d0-4b0e-82db-53defefb283f)


Después, movemos los archivos .jar y .txt a `namenode:/tmp`  

 -`docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp`  
 
 y  
 
 -`docker cp mis-dias-en-la-libreria-morisaki.txt namenode:/tmp`  

 ![image](https://github.com/user-attachments/assets/56750a37-d5bb-49c8-b02f-ea3e16a8d220)


 ### Crear carpeta de entrada dentro de `namenode` en docker  

 En la terminal, ejecutar:  

 - `docker exec -it namenode bash`

 Y luego:  
 
 - `hdfs dfs -mkdir /user/root/input_contador`

![image](https://github.com/user-attachments/assets/2970e4a5-c05a-48fd-909f-e56a4a97e72e)

### Mover libro .txt a HDFS
Nos movemos a la carpeta temporal del nodo executando en la terminal el comando  `cd /tmp`  
Después ejecutamos  `hdfs dfs -put mis-dias-en-la-libreria-morisaki.txt /user/root/input_contador`  

![image](https://github.com/user-attachments/assets/d1680a7d-97e0-4813-948e-7271d7db2648)

### Se ejecuta la tarea MapReduce  

En la terminal ejecutar el comando  `hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador`  

Para ver los resultados de nuestra tarea, ejecutamos el siguiénte comando que nos mostrará el conteo de las palabras contenidas en nuestro archivo de texto .txt  

`hdfs dfs -cat /user/root/output_contador/*`  

![image](https://github.com/user-attachments/assets/a1b8d8b8-8295-4d57-8ee0-bba619705696)

También podemos consultar los resultados en al carpeta de salida ejecutando:

 - `hdfs dfs -ls /user/root/output_contador`
![image](https://github.com/user-attachments/assets/e66550c9-3cf4-4df4-91e5-6f1810aed27d)

El archivo  `part-r-00000` contiene el conteo de palabras generado. Para exportarlo a un archivo de texto .txt, ejecutamos el comando:

 - `hdfs dfs -cat /user/root/output_contador/part-r-00000 > /tmp/libreria-morisaki_wc.txt`

![image](https://github.com/user-attachments/assets/02804526-8dc2-4901-b667-22197a72ff6f)  

Salimos ejecutando el comando:  

 - `exit`

![image](https://github.com/user-attachments/assets/4e38ae78-d807-40f7-9c00-f4118b5c5d52)

Y por útlimo, para mover los resultados del conteo de palabras en el archivo .txt a nuestra carpeta del proyecto, ejecutamos:  

 - `docker cp namenode:/tmp/libreria-morisaki_wc.txt .`

![image](https://github.com/user-attachments/assets/992d8259-713e-45ea-a41f-e46487eca7b9)

Una vez realizado este proceso aparecerá el conteo de palabras de nuestro libro en el explorador de archivos  

![image](https://github.com/user-attachments/assets/aaf33e33-99cd-4ff3-944e-ee53ad176487)













 
 


