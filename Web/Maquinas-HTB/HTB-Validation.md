
# HTB-Validation

En primer lugar, nos creamos un directorio con el nombre de la máquina desde el que trabajaremos:

    sudo mkdir Validation
    
Ahora, mediante la función <a href="../Web/Herramientas_y_Scripts/mkt.html" style="text-decoration:none">`mkt`</a> que tengo previamente definida en la `.zhshrc` crearemos nuestros directorios de trabajo:

    sudo mkt

Esta función está definida para crearnos cuatro directorios (`nmap`, `content`, `exploits` y `scripts`) desde los cuales poder trabajar a la hora de
realizar las máquinas de HTB.

## Fase de Reconocimiento:

Ejecutamos un `ping` y vemos como nos reporta un ttl=63, por tanto, ya sabemos que estamos frente una máquina Linux.

    ping -c 1 10.10.11.116
    
![Captura de pantalla -2022-07-20 10-30-17](https://user-images.githubusercontent.com/103068924/180005076-f47e79bb-ef02-40a7-8b6a-07fd7ac0f0bf.png)
    
Ahora vamos a ver mediante `whatweb` que más podemos ver:

    whatweb 10.10.11.116
    
![Captura de pantalla -2022-07-20 10-34-08](https://user-images.githubusercontent.com/103068924/180005282-38818870-b4d1-4b7e-9ab1-2212b9c86537.png)

Aquí en primer lugar nos muestra un codigo 200 de confirmación, el cual nos indica que la petición se ha realizado correctamente y tengamos un
servicio web activo. Tambien vemos que nos encontramos frente un sistema
Apache de versión 2.4.48.

Podemos ver tambien como utilizael framework de desarrollo wep `Bootstrap`.

* ¿Qué es Bootstrap? --> <a href="https://www.hostinger.mx/tutoriales/que-es-bootstrap" style="text-decoration:none">Link</a> 
    
Finalmente podemos ver que utiliza la biblioteca de JavaScript `jQuery` y utiliza la versión 7.4.23 del lenguaje de programación PHP.

Vamos a tratar de averiguar que puertos se encuentran abiertos utilizando la 
herramienta `nmap`.

# Nmap

Nos desplazamos hasta la carpeta `nmap` que hemos creado dentro de `Validatio`.

En primer lugar vamos a realizar un escaneo básico para ver que puertos TCP y UDP se encuentran abiertos, para utilizar `nmap` debemos
ser `root`:

    nmap -p- --open -T5 -vvv -n 10.10.11.116 -oG allPorts

`-p-` : Escanea todo el rango de puertos.
  
`--open` : Solo nos mostrará puertos con el estatus abierto.
             
`-T5` : Controla el tiempo y el rendimiento del escaneo donde 1 es el más lento  y 5 el más rápido.
             
`-vvv` : Verbose. Recopila los puertos abiertos por TCP y los reporta por consola.
             
`-n` : Anula la resolución DNS.

`-oG` : Exportar los resultados en formato grepeable.
  
`allPorts` : Nombre del archivo donde se guardan los resultados. Si no existe lo creará.
  
Los resultados del escaneo quedarán guardados en el archivo `allPorts`. De esta manera podremos revisar los puertos que estaban abiertos en cualquier
momento.

![Captura de pantalla -2022-07-20 14-03-41](https://user-images.githubusercontent.com/103068924/180032930-cc4431a2-1682-409b-a2f0-995e6cd4af2d.png)

El escaneo nos reporta unos cuantos puesto abiertos, vamos a ver para que sirven cada uno de ellos:

* `Puerto 22`: Por normal general este puerto se usa para conexiones seguras SSH y SFTP, siempre que no hayamos cambiado el puerto de escucha de nuestro servidor SSH.

* `Puerto 80`: Este puerto es el que se usa para la navegación web de forma no segura HTTP.

* ` Puerto 4566`: Usa el Protocolo de Control de Transmisión `TCP`. El puerto 4566 garantiza la entrega de paquetes de datos en la misma orden, en que fueron mandados. Solo cuando la conexión es determinada, los datos del usuario pueden ser mandados de modo bidireccional por la conexión. 

En este caso el puerto 4566 nos muetra el servicio `kwtc` (KWTC --> Kids Watch Time Control Service), parece ser
algún tipo de aplicación parental para niños.

* `Puerto 8080`: Es el puerto alternativo al puerto 80 TCP para servidores web, normalmente se utiliza este puerto en pruebas.









