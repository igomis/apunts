### Instal·lació de Docker

La instal·lació de docker dependrà del sistema operatiu que estem utilitzant. Ací anem a vore la que efecturan els que utilitzen linux, amb el sistema operatiu linux-mint o ubuntu , que és el que ve instal·lat en les imatges suministrades. Per a mac o windows s'haurà de mirar la pàgina web de [docker](https://www.docker.com/get-started)

Ens donem privilegis

~~~
sudo su
~~~

Utilitzant els repositoris de docker l'instal·lem:

~~~
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" | tee /etc/apt/sources.list.d/docker.list

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

apt update

apt install docker-ce docker-ce-cli containerd.io pigz
~~~

Donem permisos a l'usuari afegint-lo al grup de docker

~~~
usermod -aG docker $USER
~~~
On $USER és el teu usuari.

També haurem d'instal·lar el docker-compose

~~~
curl -L "https://github.com/docker/compose/releases/download/1.26.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
~~~

I provem

~~~
docker --version
~~~

Faltarà engegar el servei de docker per a poder executar contenidors

~~~
sudo systemctl start docker.service
~~~


**Posada en marxa d'un repositori en el IDE**

* Es clona el repositori en un directori
* Es canvia el .env.example a .env
* Es canvia la variable BASE_DIRECTORY del .env al directori utilitzat
* Es posa en marxa els contenidors amb **sh start.sh**
* La primera vola s'executa el composer amb **sh composer.sh**

Nota: Al principi de curs es canviarà el /etc/hosts per afegir la linea

> 127.0.0.1 batoi2021.my