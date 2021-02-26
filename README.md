# La finalidad es que Jellyfin funcione sobre raspberry pi 4 OS Raspbian, leyendo contenido multimedia de Gdrive soportadas las aplicaciones en docker-coompose.

#Paso 1
#Crear directorio docker y subdirectorios portainer, rclone, y jellyfin.
sudo mkdir -p docker/{portainer,rclone,jellyfin/{config,cache,media}}
cd docker

#Paso 2
#Instalación de Docker-compose 
#Se instala los paquetes necesarios: (Realizar copy paste de la línea 8 a la 16 completas e insertar en la terminal):

sudo apt-get update && sudo apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg2 \
  software-properties-common \
  vim \
  fail2ban \
  ntfs-3g

#Paso 3
#Instalar firmas GPG de los remositorios de docker:
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
 sudo apt-key fingerprint 0EBFCD88

#Paso 4
#Agregar repositorio de docker:
echo "deb [arch=armhf] https://download.docker.com/linux/debian \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list

#En este paso pude leer a peladonerd que hay versiones que daban problemas con Raspbian. Para mas info consultar el siguiente link:
#https://github.com/pablokbs/peladonerd/tree/master/raspi/1

#Paso 5
#Instalar docker
sudo apt-get update && sudo apt-get install -y --no-install-recommends docker-ce docker-compose

#Paso 6
#Agregar user al grupo docker
sudo usermod -a -G docker <usuario>
#(logout and login)
  

#hasta aquí está docker compose instalado, ahora dependiendode si quieres separar las aplicaciones que instales sobre docker en carpetas diferentes o ejecutar y añadir todas al mismo archivo .yaml . En mi caso separe las instancias creando diferentes directorios y archivos YAML.



#Paso 7
#Instalación de rclone en docker-compose
#Generamos el archivo de configuracion rclone e instalamos la versión beta con el siguiente comando en la shell:

docker run -it -v ~/.config/rclone:/config/rclone rclone/rclone:beta config

#Paso 8
#En este paso hay que configurar “remote”, “user-id" , "user-secret”, habilitar APP Drive en API Console Google y crear credenciales. Permitir acceso a nuestra cuenta gdrive desde la API y copiar el token que nos da para introducirlo en rclone.
       https://www.youtube.com/watch?v=mnDYJ2ZpdxU
       https://github.com/pablokbs/peladonerd/blob/master/v2m/26/comandos.txt
       https://rclone.org/install/
