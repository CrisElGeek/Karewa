# Despliegue y configuración del servidor para producción

## CD (Continuous Deployment)

### Objetivo

Gitlab tiene una serie de herramientas que nos automatizara la entrega y despliegue de nuestro proyecto en nuestro servidor de producción.

En esta configuración se pretende explicar las variables de entorno para el CD (Continuous Deployment)

Para agregar las variables de la configuración del CD se debe ser administrador del repositorio y se deben de configurar en las opciones de configuración del mismo desde Gitlab, es importante NO introducir el varlor de dichas variables en `.gitlab-ci.yml`, ya que es un proyecto de codigo abierto no se debe dejar a disposicion los recursos de producción.

https://docs.gitlab.com/ee/ci/variables/#via-the-ui

### Variables de configuración

En esta tabla se encuntra la definición de las variables de entorno necesarias para tener un depliegue automatico:

| Variables | Definición | Ejemplo |
| -------- | -------- |
| API_HOST_BETA   | Dominio con portocolo de beta, si es con tls se debe de especificar https | https://example.com |
| API_HOST_PROD   | Dominio con portocolo de producción, si es con tls se debe de especificar https | https://example.com |
| API_PORT_BETA | Puerto de elance del dominio de beta | 443 |
| API_PORT_PROD | Puerto de elance del dominio de producción | 443 |
| VUE_APP_API_HOST_BETA | Copia de API_HOST_BETA | https://example.com |
| VUE_APP_API_HOST_PROD | Copia de API_HOST_PROD | https://example.com |
| VUE_APP_API_PORT_BETA | Copia de API_PORT_BETA | 443 |
| VUE_APP_API_PORT_PROD | Copia de API_PORT_PROD | 443 |
| HEROKU_API_KEY | Es el token que se requiere para subir una imagen y desplegar en heroku | 00000000-1111-2222-3333-444444444444 |
| HEROKU_APP | Nombre del proyecto de heroku | example-heroku |
| HEROKU_REGISTRY | Registry de docker de heroku | registry.heroku.com/example-heroku/app |
| KAREWA_HOST_PROD | 1) Dominio de producción para desplegar producción | 175.168.15.1 |
| KAREWA_PORT_PROD | 2) Puerto ssh para desplegar producción | 2222 |
| KAREWA_USER_PROD | 3) Usuario ssh para desplegar producción | example |
| SSH_PRIVATE_KEY | 4) Llave privada ssh para acceder al servidor de producción con saltos de linea | -----BEGIN RSA PRIVATE KEY-----<br>MIIEpAIBAAKCAQEAmlq06edI4f8dGGZ/1pOBcTUmUtzWiYMIUxdyu/LuD+2j1vgi<br>GCZPaO5x4iETDdNsvbQ3HtVrmuZ9pb6mjD8FDE8jGOw+m2f1ouhvP53Xvct963h8<br>kefTzLtgI6bsf5pGkTFWf1ZRFIWqlK4/ipyGplUfEeWg5TAcIKd9gU8f96G5PNc1<br>...<br>-----END RSA PRIVATE KEY----- |
| SSH_PRIVATE_PUB | 5) Llave publica ssh para acceder al servidor de producción | ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC... |
| SMTP_ACCOUNT | Cuenta smtp que envia correos | Monitor Karewa <example@gmail.com> |
| SMTP_HOST | Dominio smtp del correo | smtp.gmail.com |
| SMTP_PASS | Contraseña del correo | password1 |
| SMTP_USER | Usuario con dominio del correo | example@gmail.com |


#### Detalles de algunas variables

1. Desde el servidor de producción creado para el monitor se debe tener acceso por ssh.
2. Puerto del servidor ssh de producción.
3. Se debe crear un usuario con acceso al grupo de docker en el servidor de producción.
4. Esta llave privada solo debe ser agrega en el CD, no es necesario tenerla almacenada.  
5. Esta llave prublica debe ser agregada como llaves autorizadas para acceder al usuario del punto (3).


### Conclusion

El archivo `.gitlab-ci.yml` define las reglas de como se debe empaquetar la aplicación, hacia donde debe ser subido y como se despliega al servidor de producción con las herramientas previamente instaladas, este mismo archivo toma las variables definidas en la configuración de la plataforma que actualemente ya estan preconfiguradas.

Si se realizó adecuadamento todos los pasos anteriores ya tendremos:
- Un servidor de producción
- Herramientas para descargar y correr el proyecto
- Una configuración de despliegue automatico a través de la plataforma de GitLab

Si se desea aprender como modificar el despliegue previamente mensionado puede apoyarse en estos enlaces para aprender a modificar el CI de GitLab.

- https://docs.gitlab.com/ee/ci/yaml/
- https://docs.gitlab.com/ee/ci/variables/
- https://docs.gitlab.com/runner/
- https://www.redhat.com/es/topics/devops/what-is-ci-cd
