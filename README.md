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
<br><br>
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

Ingresamos dentro de los contenedores *Ingresar en ambos contenedores web y ejecutar los mismos comandos*
```bash
docker exec -it web2 bash
```
![13](images/12.png)
![19](images/18.png)

Actualizamos los repositorios dentro de los contenedores
```bash
apt update
```
![14](images/13.png)

Instalamos GIT y Nano
```bash
apt install git nano -y
```
![15](images/14.png)

Ingresamos dentro de la carpeta "htdocs" y editamos el archivo "index.html"
```bash
cd htdocs
ls
nano index.html
```
![16](images/15.png)
![17](images/16.png)
![18](images/17.png)

<br><br>

Link para ingresar a GitLab
```bash
http://IP:PORT/users/sign_in
```
![20](images/gitsrv1.png)

El usuario en GitLab es "root" y la contraseña la obtenemos con el siguiente comando:
```bash
sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```
![21](images/19.png)

Seleccionamos "New project"
![22](images/gitsrv2.png)

Seleccionamos "Create blank project"
![14](images/gitsrv3.png)

Agregamos un nombre y seleccionamos el nivel de visibilidad "Public"
![15](images/gitsrv4.png)

Copiamos la dirección para clonar el repositorio
![17](images/gitsrv6.png)

Creamos un archivo el cual subiremos como prueba el repositorio.
![16](images/20.png)

Inicializamos Git y publicamos a el repositorio que creamos en GitLab
```bash
git add .
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git commit -m "Primer commit"
git push
```
![16](images/20.png)

Validamos que el archivo se encuentre en el repositorio.
<br><br>
![16](images/gitsrv7.png)

![17](images/LB1.png)
![17](images/LB2.png)
