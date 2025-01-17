# Configuración de servidor Linux con Ngix

## Requerimientos

Se recomiendan los siguiente requerimientos para el correcto funcionamiento del sistema aunque pueden variar, algunos ya estan obsoletos por lo que puede ser necesario actualizarlos

- NodeJS 12
- MongoDB 4
- Nginx

### Otras dependencias de Node necesarias

- [pm2](https://pm2.keymetrics.io/docs/usage/quick-start/)
- [yeomans 4](npm install -g yo@4.1.0) * *Es la version de yeomans que es compatible con NodeJs 12*



## Archivo de configuración de dominio

En el archivo de configuración del dominio con Nginx por ejemplo `/ect/nginx/sites-available/default` (puede variar de acuerdo al sistema o si utiliza hosts virtuales)  agregar los siguientes parámetros:

```nginx
server {
	...
	# Para permitir POST en paginas estaticas
 	error_page  405     =200 $uri;

	location /public-api {
		proxy_pass http://localhost:3000;
		roxy_set_header Host $host;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_cache_bypass $http_upgrade;
	}

	location /api {
		proxy_pass http://localhost:3000;
		roxy_set_header Host $host;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_cache_bypass $http_upgrade;
	}

	location / {
		try_files $uri $uri/ /;
	}
	...
}
```

## Instalar la aplicación en el servidor

Debemos instalar las dependencia de **VUEJS**

> npm install

## Generar los catálogos

> npm link ./generators/generator-mkw

Para crear el catálogo en MongoDB que seria nuestra base de datos. ()

> yo generator-mkw:catalog

Aqui te va a pedir el nombre del catalogo o base de datos
## Iniciar el sistema en modo producción

### PM2

#### Instalar `pm2`

[Guía completa aquí](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-20-04)

> npm install pm2@latest -g

#### Arrancar el sistema

> pm2 start app.production.sh

**Ejemplo de respuesta**

```txt
...
[PM2] Spawning PM2 daemon with pm2_home=/home/sammy/.pm2
[PM2] PM2 Successfully daemonized
[PM2] Starting /home/sammy/hello.js in fork_mode (1 instance)
[PM2] Done.
┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
│ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
│ 0  │ hello              │ fork     │ 0    │ online    │ 0%       │ 25.2mb   │
└────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘
```

#### Crear un script de inicio

> pm2 startup systemd

**Ejemplo de respuesta**

```txt
Output
[PM2] Init System found: systemd
sammy
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u sammy --hp /home/sammy
```

#### Agregar el script a tu directorio local

* Tienes que utilizar el comando que te arrojo en el paso anterior, no este

>     sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u sammy --hp /home/sammy

#### Salvar el proceso

> pm2 save

### Correr el servicio creado

> sudo systemctl start pm2-sammy

Una vez que este corriendo el servicio corremos el siguiente comando `pm2 list` y ver algo como esto:

```txt
┌────┬───────────────────┬─────────────┬─────────┬─────────┬──────────┬────────┬──────┬───────────┬──────────┬──────────┬──────────┬──────────┐
│ id │ name              │ namespace   │ version │ mode    │ pid      │ uptime │ ↺    │ status    │ cpu      │ mem      │ user     │ watching │
├────┼───────────────────┼─────────────┼─────────┼─────────┼──────────┼────────┼──────┼───────────┼──────────┼──────────┼──────────┼──────────┤
│ 1  │ APP               │ default     │ 0.2.0   │ cluster │ 46616    │ 3m     │ 1    │ online    │ 0%       │ 98.6mb   │ cri… │ disabled │
│ 0  │ app.production    │ default     │ 0.2.0   │ fork    │ 46552    │ 3m     │ 0    │ online    │ 0%       │ 3.2mb    │ cri… │ disabled │
└────┴───────────────────┴─────────────┴─────────┴─────────┴──────────┴────────┴──────┴───────────┴──────────┴──────────┴──────────┴──────────┘
```
Ahora el servicio debería estar corriendo y el sistema funcionando

* *Para conocer más utiliza el enlace que te dejamos arriba*


