# final-project

### Elementos usados:
- 1 Maquina virtual
- Ubuntu Server 22.04
- Docker 20.10.21
- Docker Compose

<br><br>

Se clona el repositorio e ingresamos a la carpeta "final-project"

```bash
git clone https://github.com/MarlonTheOne/final-project.git
ls -l
cd final-project/
```
![1](images/1.png)


*Ejemplo de descarga de Docker Compose:* [Docker Compose](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-22-04)<br><br>
![2](images/dcompos.png)

Ingresamos a la carpeta donde se encuentran los archivos de configuración
![3](images/2.png)
![4](images/3.png)
![5](images/4.png)

Descargar las imagenes para los servicios WEB y el servidor de GitLab

```bash
docker pull httpd
docker pull gitlab/gitlab-ee
```
![6](images/5.png)
![7](images/6.png)

Dentro de la carpeta "docker-lb-web" ejecutar el docker compose
```bash
docker compose up --build
```
![8](images/7.png)
![9](images/8.png)

Revisamos que se haya creado el adaptador de red (Ese adaptador se va a adjuntar al contenedor de GitLab)
```bash
docker network ls
```
![10](images/9.png)

Se despliega el último contenedor del contenedor de GitLab.
```bash
sudo docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 8080:80 --publish 21:22 \
  --network docker-lb-web_default \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ee:latest
```
![11](images/10.png)

Validamos que todos los contenedores esten en el mismo segmento

```bash
docker inspect $(docker ps -q) | grep -i "ipaddress" | grep -vE "Name|Secondary"
```
![12](images/11.png)


![13](images/12.png)
![14](images/13.png)
![15](images/14.png)
![16](images/15.png)
![17](images/16.png)
![18](images/17.png)
![19](images/18.png)
![20](images/gitsrv1.png)
![21](images/19.png)
![22](images/gitsrv2.png)
![23](images/20.png)
![24](images/21.png)
![25](images/22.png)
![14](images/gitsrv3.png)
![15](images/gitsrv4.png)
![16](images/gitsrv5.png)
![17](images/gitsrv6.png)
![16](images/gitsrv7.png)
