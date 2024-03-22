# Laravel docker
## Configuración basica para dockerizar laravel

### Instalación (Es necesario tener docker-compose instalado)

* Solo copia el contenido de este repositorio en la raiz de tu proyecto de laravel
* Usa el comando `docker-compose up -d --build --remove-orphans` 

Puedes hacer un archivo bash para que sea más sencillo:

```bash
#!/bin/bash
echo "Removing old containers in case they exists"
docker stop $(docker ps -aq)
docker rmi $(docker images -a -q) -f
docker rm $(docker ps -aq)
docker system prune -f

echo "Initializing the new containers"
docker-compose up -d --build --remove-orphans

echo "Ready to work..."
# docker logs --follow ${APP_NAME}-container
```