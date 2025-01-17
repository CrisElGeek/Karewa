# Despliegue y configuración del servidor para producción

## Google Cloud Platform (GCP)

### Objetivo
En eta sección se pretende mostrar como crear una instancia con GCP para producción.
Se debe tener un cuenta con GCP previamente.

### Creación de la instancia
1. Después de haber iniciado sesión en nuestro proyecto GCP.
2. Antes de crear una instancia asegurar de que exista el firewall habilitado de "default-allow-ssh". Para poder entrar por ssh. En dado caso de no tener, crear la regla aplicando para todas las instancias de la siguiente manera:
    1. Seleccionar el recurso "Firewall rules", dentro de "VPC network". Del menú de la izquierda.

    <center><img src="./docs/images/PasoParaFirewall.png" height="200" width="250"></center>

    2. Seleccionar "CREATE FIREWALL RULE".

    <center><img src="./docs/images/PasoSeleccionarFirewall.png" height="30" width="300"></center>

    3. Hacer la configuración general para firewall.
        1. Agregar nombre a Firewall.
        2. Dirección de trafico "ingress".
        3. En la parte de "Targets", seleccione en "All instances in the network".
        4. En "source filter", debe ser 0.0.0.0/0, para que se puedas entrar de diferentes ip.
        5. En "Protocol and ports" debe ser por tcp y puerto 22.

Al final damos "create".

<center><img src="./docs/images/PasoConfigFirewall.png" height="350" width="300"></center>

2. Seleccionar el recurso "VM Instances", dentro de "Compute Engine". Del menú de la izquierda.

<center><img src="./docs/images/1.PasoSeleccionVMInstances.png" height="310" width="250"></center>

3. Seleccionar "CREATE INSTANCE".

<center><img src="./docs/images/2.PasoSeleccionCreateInstance.png" height="50" width="700"></center>

4. Dentro de este se abrirá la opción para poder configurar la instancia. Configurando de la siguiente manera:
    1. Introducir nombre de instancia.
    2. Introducir región y zona de instancia. Esto toma efecto solo sobre el data center seleccionado. Como recomendación para seleccionar el adecuado seleccionar el mas cercano a los usuarios.
    3. Introducir tipo de maquina. En este paso podemos seleccionar varios tipos preconfigurado por Google o personalizarlo. Elegir el mas adecuado para la tarea.
    4. Seleccionar sistema operativo ha instalar en instancia.
    5. Seleccionar permitir trafico por HTTP y HTTPS.  Si se permite acceso por http y https es para que cualquier servicio que trabaje sobre estos protocolos en la "instancia VM" funcione hacia el exterior. Para configurar otros puertos se debe configurar tu propia regla de firewall, como se hizo en el paso 2.
    6. Para finalizar seleccionamos "Create".

<center><img src="./docs/images/3.PasoConfiguracionInstance.png" height="350" width="250"></center>

7. Seleccionar el recurso "External IP addresses", dentro de "VPC network". Del menú de la izquierda

  <center><img src="./docs/images/4.PasoSeleccionarExternalIp.png" height="200" width="250"></center>

8. Seleccionar "RESERVE STATIC ADDRESS"

<center><img src="./docs/images/5.PasoSeleccionarReservaIP.png" height="50" width="700"></center>

9. Configurar la ip externa de la siguiente manera:
    1. Dar un nombre identificable a tu ip externa.
    2. Seleccionar el tipo de ip, en este caso queda como IPv4.
    3. Seleccionar Región, que sea en la misma zona donde se creo la instancia.
    4. Seleccionar la instancia creada anteriormente.
    5. Seleccionar "Reserve".


<center><img src="./docs/images/6.PasoConfiguracionIp.png" height="300" width="300"></center>

10. Una vez creado la instancia y reservada la ip, la llave ssh debe de estar previamente configurada para que se asigne sola a la instancia.

### Conclusion
Si realizamos correctamente todos los pasos del punto anterior ya habremos creado una instancia con GCP, podremos comprobar entrado via ssh desde nuestra terminal "<user\>@<ip\>", ejemplo:

```bash
ssh foo@127.0.0.1
```
