# Despliegue y configuración del servidor para producción

## Docker Engine y Docker Compose

### Objetivo
En esta sección se indicara como realizar una instalacion correcta de docker engine y docker compose las cuales son un para de herramientas que nos facilitara correr el proyecto en producción y tener un depliegue automatico con el CD de GitLab.

### Instalación

1. Para poder instalar docker-compose primero debemos de instalar docker ce, para esto seguimos los siguientes pasos:
    1. Para instalar docker en debian ya que es la imagen que hemos instalado para este ejemplo, hacer lo siguiente (si seleccionaste otra sistema <a href="https://docs.docker.com/install/linux/docker-ce/ubuntu/">Aquí más información</a>):
    2. Actualizar paquetes:
    ```bash
    $ sudo apt-get update
    ```
    3. Instalar paquetes para permitir apt usar repositorio por HTTPS:
    ```bash
    $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common
    ```
    4. Agregar la llave GPG oficial de docker:
    ```bash
    $ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
    ```
    5. Verificar que tu tengas la llave con fingerprint ```9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88```, buscando los últimos 8 caracteres de el fingerprint.
    ```bash
    $ sudo apt-key fingerprint 0EBFCD88
    pub   4096R/0EBFCD88 2017-02-22
    Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
    uid                  Docker Release (CE deb) <docker@docker.com>
    sub   4096R/F273FCD8 2017-02-22
    ```
    6. Usa el siguiente comando para configurar repositorio estable.
    ```bash
    $ sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/debian \
    $(lsb_release -cs) \
    stable"
    ```
    7. Actualiza los paquetes para que surgan cambios en los repositorios agregados.
    ```bash
    $ sudo apt-get update
    ```
    8. Instala la versión latest de docker.
    ```bash
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```
    9. Verifica que se haya instalado correctamente.
    ```bash
    $ sudo docker run --rm hello-world
    ```
    10. Para poder usar el comando docker con tu usuario, necesitas pertenecer al grupo "docker".
    ```bash
    $  sudo usermod -aG docker <usuario>
    ```
    11. Salir de ssh y volver a entrar para poder ejecutar el comando sin sudo.
2. Por ultimo lo que se debe de hacer es instalar docker-compose de la siguiente manera:

    1. Instalar versión estable de Docker Compose.
    ```bash
    $ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
    2. Aplicar permisos de ejecución a el binario.
    ```bash
    $ sudo chmod +x /usr/local/bin/docker-compose
    ```
    3. Probar instalación.
    ```bash
    $ docker-compose --version
    ```

### Conclusion
Hasta este punto ya contamos con una instancia de GCP con las herramientas necesarias podremos prerpar el despliegue (deploy) del proyecto en producción
