

# M347 Dokumentation



## Grundlagen

**Containerisierung** ist sozusagen die Virtualisierung mit Hilfe von **Containern**. Die einzelnen Container sind laufende Rechenumgebungen.

​					![VM vs containers](https://cdn.hashnode.com/res/hashnode/image/upload/v1596298477648/alMXfcmFx.png?auto=compress,format&format=webp)



![Virtuelle Maschinen vs. Container](https://gbssg.gitlab.io/m347/img/kap1/1-1.PNG)



Wenn wir einen Container starten haben wir einen bestimmten Prozess am Laufen, bei einer VM muss immer ein ganzes Betriebssystem laufen. Hauptunterschied ist also, dass Container weniger Ressourcen benötigen. Ebenfalls sind Container auf jedem Betriebssystem verfügbar. Docker engine übernimmt die Verteilung der Ressourcen.



Hier einige wichtige Begriffe

| Begriff    | Definition                                                   |
| :--------- | :----------------------------------------------------------- |
| Image      | Schreibgeschützte Vorlage mit dem Speicherabbild eines Containers. |
| Container  | Aktive Instanz eines Images.                                 |
| Layer      | Teil eines Images. Enthält einen Befehl oder eine Datei, die dem Image hinzugefügt wurde. |
| Repository | Satz gleichnamiger Images mit verschiedenen Tags, zumeist Versionen. |
| Registry   | Verwaltet Repositories. z.B. DockerHub                       |
| Dockerfile | Textdatei mit allen Befehlen, um ein Image zusammenzustellen. |



Der Client ist z.B. eine Konsole. Das Betriebssystem kann als DOCKER_HOST bezeichnet werden. Im DOCKER_HOST hat der Docker daemon das Sagen. Registry ist vergleichbar mit Git wo man Images pusht und pullt.

![Docker-Architektur](https://gbssg.gitlab.io/m347/img/kap1/docker-architecture.svg)



## Konzepte



### Image

Ein Image kann als eine Art von "virtuellem Container" betrachtet werden, der es ermöglicht, Anwendungen oder Betriebssysteme auf verschiedenen Computern oder Servern auszuführen, ohne dass es notwendig ist, jedes Mal eine separate Installation durchzuführen. Das bedeutet, dass ein Image in einem Docking-System verwendet werden kann, um eine Anwendung oder ein Betriebssystem schnell bereitzustellen, ohne dass es notwendig ist, es auf jedem einzelnen Computer oder Server zu installieren.



### Microservices

Jeder Container hat eine spezifische Aufgabe. Man spricht deshalb von Microservices.



### Orchestrierung

Mithilfe der Container-Orchestrierung werden Deployment, Management, Skalierung und Vernetzung von Containern automatisiert. Heisst es hilft Container zu verwalten.



## Container ausführen



**Image herunterladen**

```bash
docker pull ubuntu:latest
```

**Container starten**

```bash
docker run -it --name my-ubuntu-container ubuntu:latest
```

Um zu überprüfen, **welche Container gestartet** sind können Sie docker ps (2. Terminal) verwenden:

```bash
docker ps
```

**Container stoppen**

```bash
docker stop my-ubuntu-container
```

**Container Löschen**

`docker rm` löscht einen gestoppten Container, das Image bleeibt jedoch vorhanden.

```bash
docker rm my-ubuntu-container
```

**Image löschen**

```bash
docker rmi ubuntu:latest
```



**Image Namen**

Docker-Image-Namen setzen sich aus drei Teilen zusammen:

```bash
source/imagename:tag
```

- **source** gibt den Namen der Organisatio (oder Person) an, die das Image erstellt hat, z.B. docker
- **imagename** der Name des Images, z.B. getting-started
- **tag** die Versionsnummer des Images, z.B. 22.04

Wird keine source angegeben, nimmt docker an, dass eines der offiziellen Dockerimages gemeint ist. Wird kein tag angegeben, wird automatisch das tag latest verwendet.



![1.4.1](https://gbssg.gitlab.io/m347/img/kap1/4-1.PNG)



**Docker pull**

```bash
docker pull nginx
```

**Docker stop**

```bash
docker stop my-nginx-container
```

**Docker start**

```bash
docker start my-nginx-container
```

**Docker rm**

```bash
docker rm my-nginx-container
```

**Docker rmi**

Zweck: Ein Image auf dem Host wird gelöscht. Es dürfen keine abgeleiteten Container von diesem Image vorhanden sein (weder laufend noch gestoppt)

```bash
docker rmi nginx
```

**Docker ps**

Zeigt laufende Container, wenn -a auch noch gestoppte.

```bash
docker ps -a
```

**Docker images**

```bash
docker images
docker image ls
```



## Portweiterleitungen

Stellt ein Container einen Serverdienst über einen bestimmten Port zur Verfügung kann dieser mit einem anderen Port (oder auch dem gleichen) Port auf dem Host verknüpft werden.

Läuft beispielsweise im Container eine Webanwendung auf Port 80 (http), läss sich dieser Port beim Start des Containers mit dem Parameter -p mit dem Port 80 des Hostrechners so verbinden

```bash
docker run  -p 80:80 <image>
```

Auf dem Host lässt sich somit die Webseite des Containers mit `http://localhost` öffnen

Die Syntax für -p lautet

```
-p hostport:containerport
```



Für Webdienste üblich ist z.B. Port 8080:

```
docker run  -p 8080:80 <image>
```

Die Containerwebseite ist dann unter `http://localhost:8080` erreichbar.



### Aufgabe Portweiterleitung

Auf welchem Port läuft die Webseite im Container?

Auf dem Container-Port

Starten Sie einen Container aus getting-started, der auf dem Host auf http://localhost:8080 erreichbar ist

1.  "getting-started" herunterladen

   ``docker pull docker/getting-started``

   ![image-20230423170615217](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423170615217.png)

2. Container starten. Mit -p den Host-Port 8080 und den standardmäßigen Container-Port von 80 angeben

   ```bash
   docker run -p 8080:80 docker/getting-started
   ```

![image-20230423171331127](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423171331127.png)

![image-20230423171416886](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423171416886.png)



Beenden Sie am Schluss den Container, löschen Sie ihn und auch das Image.

```bash
docker stop b967ea729bcd

docker rm b967ea729bcd

docker images
docker rmi 3e4394f6b72f
```

![image-20230423173050965](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423173050965.png)

![image-20230423173204541](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423173204541.png)

![image-20230423173130239](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423173130239.png)

![image-20230423173222526](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230423173222526.png)



## Volumes

Sollen Speicherort der daten, die in einem Container verwendet werden, ändern. Sie werden nämlich standardmässig im Container gespeichert, was heisst, dass bei einer Neuinstallation des Containers alle Daten weg sind.

Hier geht es darum, wo ein Container seine Daten speichert. Das [Docker-Zustandsdiagramm](https://gbssg.gitlab.io/m347/docker-wichtigeKommandos) legt nahe, dass Daten die in einem laufenden Container gespeichert sind, erhalten bleiben, wenn der Container gestoppt und wieder gestartet wird, nicht jedoch wenn ein Container gelöscht und wieder neu erzeugt wird.

Um das zu demonstrieren verwenden wir einen Container für den mariadb-Server: Wir erzeugen einen Container (-d bewirkt, dass der Container im Hintergrund läuft, ausserdem muss ein root-Passwort gesetzt werden):

```bash
docker run -d --name mariadb-test -e MYSQL_ROOT_PASSWORD=geheim mariadb:latest
```

Öffnen Sie eine root-Shell innerhalb des Containers mit

```bash
docker exec -it mariadb-test /bin/bash
```

dann legen wir mit touch eine neue Datei abc.txt an

```bash
vmadmin@ubuntu:~$ docker exec -it mariadb-test /bin/bash
root@5de9ed33a981:/# touch abc.txt
root@5de9ed33a981:/# ls
abc.txt  bin ...
root@5de9ed33a981:/# exit
```



wenn man jetzt diese Commands ausführt sind die Daten immer noch vorhanden (bzw. die Datei)

```bash
docker stop mariadb-test
docker start mariadb-test
```



wenn man den Container allerdings löscht und neu erstellt ist es nicht mehr vorhanden.

```bash
docker stop mariadb-test
docker rm mariadb-test
docker run -d --name mariadb-test -e MYSQL_ROOT_PASSWORD=geheim mariadb:latest
```



### Unbenannte Volumes

```bash
docker run -d --name mariadb-test -e MYSQL_ROOT_PASSWORD=geheim mariadb
```

Um herauszufinden wo nun die Daten aus /var/lib/mysql gelandet sind können Sie das Kommando

```bash
docker inspect -f '{{.Mounts}}' mariadb-test
```

Das Resultat daraus wäre:

```bash
vmadmin@ubuntu:~/bsp-apache-php$ docker inspect -f '{{.Mounts}}' mariadb-test
[{volume 752507751a42f0c781b96adacb4a3d73bdbbf2184ead3bd4874b4b5f065ee4eb /var/lib/docker/volumes/752507751a42f0c781b96adacb4a3d73bdbbf2184ead3bd4874b4b5f065ee4eb/_data /var/lib/mysql local  true }]
vmadmin@ubuntu:~/bsp-apache-php$
```

Wenn beim Erstellen des Containers also nichts eingestellt wird, liegt das Verzeichnis in einem zufällig benannten Unterverzeichnis von /var/lib/docker/volumes des Hostrechners. Die zufällige Bezeichnung verunmöglicht eine praktikable Verwaltung von Volumes nahezu. Sollte z.B ein Volume gelöscht werden, müsste der vollständige Verzeichnisname angegeben werden: `docker volume rm 7525077...`

Eine Wiederverwendung des Volumes nach einem Löschen des Containers ist ebenfalls nicht möglich, da bei jedem Neuerstellen einens Containers eine neue zufällige Bezeichnung des Volumes erzeugt wird, d.h. die Daten sind nach einem Upgrade auf eine neuere Version nicht mehr vorhanden.



### Benannte Volumes

```bash
docker run -d --name mariadb-test2 -v myvolume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=geheim mariadb
```

Auf der Linux-Konsole sieht dies so aus:

```bash
vmadmin@ubuntu:~/bsp-apache-php$ docker run -d --name mariadb-test2 -v myvolume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=geheim mariadb
2ccb55f675a5867efeefaa8bfd02e896cf0e0e1b66fe098465d6d7cc3e6a9bc6
vmadmin@ubuntu:~/bsp-apache-php$ docker inspect -f '{{.Mounts}}' mariadb-test2
[{volume myvolume /var/lib/docker/volumes/myvolume/_data /var/lib/mysql local z true }]
```

Die Syntax für ein Benanntes Volume ist also:

```bash
-v volumename:containerverzeichnis
```



### Volumes in eigenen Verzeichnissen

Anstelle eines Namens für das Hostvolume kann auch ein Verzeichnis angegeben werden die Syntax dazu lautet:

```bash
-v hostverzeichnis:containerverzeichnis
```

Damit wird das Volume ganz aus der Dockerumgebung herausgelöst und kann an einer beliebigen Stelle platziert werden:

```bash
mkdir /home/vmadmin/databases
docker run -d --name mariadb-test3 -v /home/vmadmin/databases:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=geheim mariadb
```



### Aufgabe Volumes

• Starten Sie drei Container aus dem mariadb-Image und verbinden Sie die Containerports mit den beiden Hostports 3306, 3307 und 3308.
• Das Rootpasswort kann mit dem Parameter -e angegeben werden: -e MYSQL_ROOT_PASSWORD=geheim
• Verwenden Sie im ersten Container ein unbenanntes Volumen für die Datenbanken 

• Verwenden Sie im zweiten Container ein benanntes Volumen 

• Verwenden Sie im dritten Container ein Volumen, welches auf dem Hostrechner in /home/vmadmin/mysql liegt. (Der Ordner muss zuerst mit mkdir erstellt werden) • Lassen Sie sich in allen Fällen den Inhalt des Volumes mit ls -l anzeigen
• Installieren Sie auf dem Host den mysql-client mit sudo apt install mariadb-client • Verbinden Sie sich bei beiden Containern mit mysql -u root -p -P 3306 -h 127.0.0.1 mysql -u root -p -P 3307 -h 127.0.0.1 mysql -u root -p -P 3308 -h 127.0.0.1 und legen Sie je eine neue Datenbank an. (-h 127.0.0.1 ist nötig um TCP zu erzwingen) 

• Stoppen und löschen Sie nun die Container (Simulation eines Upgrades vom mariadb)
• Erstellen Sie drei neue Container mit den selben Volumes und Ports wie vorher. 

• Überprüfen Sie ob die zuvor erstellten Datenbanken noch vorhanden sind.
• Löschen Sie alle Container, Images und Volumes



![image-20230424110528882](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230424110528882.png)

![image-20230424111231018](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230424111231018.png)



## Netzwerke

Hier geht es darum wie mehrere Dockercontainer untereinander kommunizieren können. Beispielsweise muss ein Web-Container mit einem Datenbank-Container kommunizieren können und an seine Daten zu kommen.

vorhandenen Netzwerke:

```bash
docker network ls
```

```bash
vmadmin@ubuntu:~$ docker network ls
NETWORK ID     NAME                                 DRIVER    SCOPE
65593a9ebb3b   bridge                               bridge    local
364521a9eaa2   host                                 host      local
c69c18f0a974   none  
```



### Standardnetzwerk

Das Netzwerk mit dem Namen bridge ist das Standardnetzwerk und wird verwendet wenn nichts anderes angegeben wird. Die Netzwerkarchitektur lässt sich wie folgt darstellen:

![1.7.1](https://gbssg.gitlab.io/m347/img/kap1/7-1.PNG)

Folgendes um diese Architektur nachvollziehen zu können:

```bash
vmadmin@ubuntu:~$ ip addr
...
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c3:53:04:66 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0

...
```



```bash
docker network inspect bridge
```



### Eigene Netzwerke

Alle Container landen standardmässig im selben Netzwerk, dem bridge-Netzwerk. Dies ist aus sicherheitstechnischen Gründen nicht ideal, wenn unterschiedliche Anwendungen voneinander isoliert sein sollen. Es lassen sich deshalb eigene Netzwerke definieren und diese den Containern zuordnen.

```bash
docker network create \
--driver=bridge \
--subnet=10.10.10.0/24 \
--gateway=10.10.10.1 \
my_net
```

Ein Container kann nun beim Start diesem Netzwerk zugeordnet werden, indem man dies ausführt:

```bash
docker run -it --name ubuntu_2 --network=my_net ubuntu:latest
```

Um die IP anzuzeigen:

```bash
docker network inspect my_net
```

Die IP-Adresse für den Container wird dabei von docker via DHCP aus dem definierten Netzwerk vergeben. Alternativ kann eine fixe IP-Adresse beim Start des Containers angegeben werden.

```bash
docker run -it --name ubuntu_2 --ip="10.10.10.10" --network=my_net ubuntu:latest
```



Als nächstes soll nun der weiteroben dem Netzwerk bridge zugeordnete Container ubuntu_1 dem Netzwerk my_net zugeordnet werden. Dazu trennen wir ihn zuerst von bridge mit

```bash
docker network disconnect bridge ubuntu_1
```

anschliessend wird er zu my_net hinzugefügt und neu gestartet

```bash
docker network connect my_net ubuntu_1
docker start -i ubuntu_1
```

Um zu überprüfen, ob die beiden Container tatsächlich mit einander kommunizieren können, installieren wir auf ubuntu_1 das Paket iputils-ping:

```bash
apt update
apt install iputils-ping
```

Dabei ist es nicht einmal nötig die IP-Adresse von ubuntu_2 zu kennen, da man auch den Namen direkt verwenden kann

```bash
root@5fe876094647:/# ping ubuntu_2
PING ubuntu_2 (10.10.10.2) 56(84) bytes of data.
64 bytes from ubuntu_2.my_net (10.10.10.2): icmp_seq=1 ttl=64 time=0.099 ms
...
```

Netzwerk löschen mit:

```bash
docker network rm my_net
```



### Aufgabe Netzwerke

• Definieren Sie das docker-Netzwerk 192.168.100.0/24 mit Gateway 192.168.100.1

```bash
docker network create --driver=bridge --subnet=192.168.100.0/24 --gateway=192.168.100.1 my-network
```

![image-20230424121047312](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230424121047312.png)

<img src="C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230424121155633.png" alt="image-20230424121155633" style="zoom: 33%;" />



• Starten Sie 2 ubuntu-Container und ordnen Sie diese dem oben erstellten Netzwerk zu: Der erste Container soll seine IP-Addresse via DHCP erhalten. Der zweite soll die IP-Adresse
192.168.100.100 erhalten

```bash
docker run -it --name ubuntu_1 --network=my-network ubuntu:latest
```

```bash
 docker run -it --name ubuntu_2 --ip="192.168.100.100" --network=my-network ubuntu:latest
```

<img src="C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230424122457559.png" alt="image-20230424122457559" style="zoom:50%;" />

Die Container sollen sich gegenseitig anpingen können

```bash
apt update
apt install iputils-ping
apt install net-tools
```

```bash
ping ubuntu_2
```

![image-20230429161210557](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429161210557.png)

![image-20230429161407377](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429161407377.png)

Stoppen und Löschen sie alles wieder

![image-20230429161708429](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429161708429.png)

```bash
docker network rm my-network
```



### Einrichten einer Wordpress-Applikation

Definieren Sie das docker-Netzwerk wp_net 192.168.200.0/24 mit Gateway 192.168.200.1

```bash
docker network create --driver=bridge --subnet=192.168.100.0/24 --gateway=192.168.100.1 wp_net
```

![image-20230429162209959](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429162209959.png)

Starten Sie einen mariadb-Container in diesem Netzwerk:

```bash
docker run -d --name wp_mariadb --network wp_net -e MYSQL_ROOT_PASSWORD=strenggeheim -e MYSQL_DATABASE=wp -e MYSQL_USER=wpuser -e MYSQL_PASSWORD=geheim -v wp_dbvolume:/var/lib/mysql mariadb
```

![image-20230429162817740](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429162817740.png)



Starten Sie einen phpmyadmin-Container ebenfalls in diesem Netzwerk:

```bash
docker run -d --name wp_pma --network wp_net -p 8080:80 -e PMA_HOST=wp_mariadb phpmyadmin/phpmyadmin
```

![image-20230429163106263](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429163106263.png)

Starten Sie nun den wordpress-Container mit:

```bash
docker run -d --name wp_wordpress --network wp_net -v wp_htmlvolume:/var/www/html/wp-content -p 8081:80 -e WORDPRESS_DB_HOST=wp_mariadb -e WORDPRESS_DB_USER=wpuser -e WORDPRESS_DB_NAME=wp -e WORDPRESS_DB_PASSWORD=geheim wordpress
```

![image-20230429164039109](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429164039109.png)

![image-20230429164009600](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429164009600.png)

IP-Adressen der drei Container:

![image-20230429164304586](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230429164304586.png)



## Images

### Standardimages

Das **Alpine**-Image ist im Gegensatz zu anderen Images klein und ist deshalb auch schnell gestartet.

| Distribution | Image Grösse |
| :----------- | ------------ |
| alpine       | 6 MB         |
| ubuntu       | 80 MB        |
| debian       | 125 MB       |
| oracle       | 250 MB       |

Standardmässig kommt die rudimentäre `/bin/sh` Shell zum Zug. Bash lässt sich aber nachinstallieren:

```shell
apk add --update bash bash-completion
```

So probiert man es aus:

```bash
docker run -it --rm -h alpine --name alpine alpine
```

Man gelangt in eine interaktive root-Shell und kann z.B. die Version abfragen:

```bash
cat /etc/os-release
```



**Ubuntu** lässt sich starten mit:

```bash
docker run -it --name ubuntu-test ubuntu:latest
```



Das offizielle Image von **apache** enthält nur den apache http Server, also kein php. Um den Server zu testen können Sie dieses Kommando verwenden:

```bash
docker run -dit --name my-apache-app -p 8080:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:2.4
```

Dieses verbindet das lokale Arbeitsverzeichnis ($PWD) mit dem Serverroot. Die Webseite ist unter http://localhost:8080 verfügbar.



**mariadb** wird von vielen Applikationen als Datenbankserver verwendet. Sie können ein mariadb Container starten mit:

```bash
mkdir dbvolume
docker run -d --name mariadb -e MYSQL_ROOT_PASSWORD=geheim \
-v $(pwd)/dbvolume:/var/lib/mysql mariadb
```

Dabei wird über eine Umgebungsvariable das root-Passwort gesetzt und das Datenbankverzeichnis im Container /var/lib/mysql auf das aktuelle Arbeitsverzeichnis/dbvolume gemountet.



Weitere Umgebungsvariablen für dieses Image wären:

| Variable                   | Bedeutung                                                    |
| :------------------------- | :----------------------------------------------------------- |
| MYSQL_ROOT_PASSWORD        | Das mysql root Passwort                                      |
| MYSQL_DATABASE             | Wenn diese Variable gesetzt ist, wird beim ersten Start des Containers eine leere Datenbank mit dem angegebenen Namen erstellt. |
| MYSQL_USER                 | Der angegebene Benutzer wird beim ersten Start des Containers erstellt und erhält alle Rechte (GRANT ALL) auf der in MYSQL_DATABASE angegeben Datenbank. |
| MYSQL_PASSWORD             | Das Passwort für MYSQL_USER                                  |
| MYSQL_ALLOW_EMPTY_PASSWORD | Wenn diese Variable auf yes gesetzt wird, kann bei MYSQL_ROOT_PASSWORD ein leeres Passwort gesetzt werden. Dies ist nicht zu empfehlen. |
| MYSQL_RANDOM_ROOT_PASSWORD | Wird diese Variable aus yes gesetzt, wird ein zufällig erzeugtes root Passwort gesetzt. |



### Eigene Images

Kurz gesagt wird in einem Arbeitsverzeichnis die Datei mit Namen **Dockerfile** erstellt. Diese enthält in einer speziellen Syntax das Rezept um das Image anschliessend mit `docker build` zu erstellen. Mit `docker push` kann das Image bei Bedarf in den Dockerhub geladen werden.



**Erstes Beispiel**

Bei diesem Beispiel wird ein Image erstellt, welches einen Webserver startet und eine Seite ausliefert mit dem aktuellen Datum und Zeit.

```bash
cd bsp-apache-php
```

Erstellen Sie in diesem Verzeichnis eine neue Datei mit Namen Dockerfile und dem abgebildeten Inhalt:

```bash
# Datei: bsp-apache-php/Dockerfile
FROM php:8-apache
COPY index.php /var/www/html
```



Diese Datei gibt das Rezept an, wie ein Image erstellt wird. **FROM** gibt das verwendete Basisimage an. **COPY** kopiert die Datei index.php ins Verzeichnis /var/www/html im Container. Dieses Verzeichnis ist bei Apache das Standardverzeichnis für Webseiten, d.h. beim Aufruf der Webseite wird index.php aufgerufen von php bearbeitet und an den aufrufenden Browser ausgeliefert.

Die Datei index.php muss natürlich noch erstellt werden und hat folgenden Inhalt:

```php
<!DOCTYPE html >
<!-- Datei index.php -->
<html >
<head >
<title >Beispiel</title >
<meta charset ="utf-8" />
</head >
<body >
<h1>Beispiel apache/php</h1>
Serverzeit : <?php echo date("j. F Y, H:i:s, e "); ?>
</body >
</html >
```

Nun wird das Image mit folgendem Befehl erstellt:

```bash
docker build -t bsp-apache-php .
```

Überprüfen Sie den Erfolg mit dem Kommando:

```bash
docker image ls
```

Hier sollte (unter anderem) nun 2 Images aufgeführt werden:

```
REPOSITORY       TAG        IMAGE ID       CREATED              SIZE
bsp-apache-php   latest     c5a049e80c58   About a minute ago   458MB
php              8-apache   af944036d594   2 days ago           458MB
```

Nun kann aus dem Image ein Container gestartet werden:

```bash
docker run -d --name bsp-apache-php-container -p 8080:80 bsp-apache-php
```

Nun können Sie die Webseite http://localhost:8080 aufrufen



**Zweites Beispiel**

Hier sehen Sie ein etwas umfangreicheres Beispiel eines Dockerfiles für ein eigenes Webserver Image:

```bash
# Datei Dockerfile
FROM ubuntu:latest

LABEL maintainer "name@meine-webseite.ch "
LABEL description "Webserver Image für www.meine-webseite.ch"

# Umgebungsvariablen und Zeitzone einstellen
# (erspart interaktive Rückfragen )
ENV TZ="Europe/Zuerich" \
    APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2

# Zeitzone einstellen, Apache installieren, unnötige Dateien
# aus dem Paket-Cache gleich wieder entfernen, HTTPS aktivieren
RUN ln -snf /usr/share/zoneinfo/$TZ  /etc/localtime && \
    echo $TZ > /etc/timezone && \
    apt-get update && \
    apt-get install -y apt-utils apache2 && \
    apt-get -y clean && \
    rm -r /var/cache/apt /var/lib/apt/lists/* && \
    a2ensite default-ssl && \
    a2enmod ssl

# Ports 80 und 443 freigeben
EXPOSE 80 443

# Das Webroot-Verzeichnis als Volume definieren
VOLUME /var/www/html

# Startkommando für den Apache Webserver
CMD ["/usr/sbin/apache2ctl" , "-D" , "FOREGROUND"]
```

Das Image wird nun erstellt mit

```bash
docker build -t meine-webseite-image
```

Ein aus dem Image abgeleiteter Container kann nun mit folgendem Kommando gestartet werden

```bash
docker run -d --name meine-webseite-container -p 8888:80 \
-v /home/vmadmin/meine-webseite/site:/var/www/html meine-webseite-image
```

Das Verzeichnis site muss vorgängig erstellt werden und wird auf das Webroot-Verzeichnis abgebildet. Wenn Sie darin eine Indexdatei index.html erstellen, wird diese nun beim Aufruf von http://localhost:8888 angezeigt

### Aufgabe

![image-20230501103448439](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230501103448439.png)

![image-20230501103652337](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230501103652337.png)

![image-20230501110257692](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230501110257692.png)

![image-20230501110809183](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230501110809183.png)

Was ist der Unterschied zwischen COPY und ADD

Der `ADD`-Befehl ist ähnlich dem `COPY`-Befehl, hat aber einige zusätzliche Funktionen. Der `ADD`-Befehl kann auch URLs als Quelle für das Kopieren von Dateien akzeptieren und kann auch Archive (wie `.tar` oder `.zip`) extrahieren und in das Docker-Image kopieren. Der Befehl hat das folgende Format:

Unterschied zwischen CMD und ENTRYPOINT

`CMD` definiert standardmäßig den Befehl oder die Anwendung, die beim Starten des Containers ausgeführt wird.

Wenn der Docker-Container beim Starten ausgeführt wird, kann das `CMD` überschrieben werden, indem ein anderer Befehl als Argument an das `docker run`-Kommando übergeben wird.

Im Gegensatz dazu definiert `ENTRYPOINT` den ausführbaren Befehl, der beim Start des Containers ausgeführt wird. 



## Docker compose



### Docker compose Aufgabe 1

docker-compose.yml:

```bash
services:
  wp_mariadb:
    image: mariadb:latest
    networks:
    - wp_net
    environment:
    - MYSQL_ROOT_PASSWORD=strenggeheim
    - MYSQL_DATABASE=wp
    - MYSQL_USER=wpuser
    - MYSQL_PASSWORD=geheim
    volumes:
    - wp_dbvolume:/var/lib/mysql

  wp_wordpress:
    image: wordpress:latest
    networks:
    - wp_net
    environment:
    - WORDPRESS_DB_HOST=wp_mariadb
    - WORDPRESS_DB_USER=wpuser
    - WORDPRESS_DB_NAME=wp
    - WORDPRESS_DB_PASSWORD=geheim
    volumes:
	- wp_htmlvolume:/var/www/html/wp-content
    ports:
    - "8081:80"

networks:
  wp_net:
    ipam:
      driver: default
      config:
        - subnet: 192.168.200.0/24
          gateway: 192.168.200.1

volumes:
  wp_dbvolume:
  wp_htmlvolume:
```



![Docker compose](C:\Users\incre\Downloads\Docker compose.png)

### Docker compose Aufgabe 2

docker-compose.yml:

```bash
version: "3.8"

services:
 mariadb:
  image: mariadb:latest
  container_name: mymariadb
  volumes:
  - wp_dbvolume:/var/lib/mysql
  environment:
  - WORDPRESS_DB_HOST=wp_mariadb
  - WORDPRESS_DB_USER=wpuser
  - WORDPRESS_DB_NAME=wp
  - WORDPRESS_DB_PASSWORD=geheim
  ports:
  - "8081:80"
volumes:
 wp_dbvolume:
```



![image-20230515113513910](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230515113513910.png)



### Aufgabe 4.3-2

File erstellen

```bash
nano docker-compose.yml
```

docker-compose Inhalt

```dockerfile
version: '3.8'
services:
  wp_mariadb:
    image: mariadb
    container_name: wp_mariadb
    networks:
      - wp_net
    environment:
      - MYSQL_ROOT_PASSWORD=strenggeheim
      - MYSQL_DATABASE=wp
      - MYSQL_USER=wpuser
      - MYSQL_PASSWORD=geheim
    volumes:
      - wp_dbvolume:/var/lib/mysql

  wp_pma:
    image: phpmyadmin/phpmyadmin
    container_name: wp_pma
    networks:
      - wp_net
    ports:
      - 8080:80
    environment:
      - PMA_HOST=wp_mariadb

  wp_wordpress:
    image: wordpress
    container_name: wp_wordpress
    networks:
      - wp_net
    volumes:
      - wp_htmlvolume:/var/www/html/wp-content
    ports:
      - 8081:80
	environment:
      - WORDPRESS_DB_HOST=wp_mariadb
      - WORDPRESS_DB_USER=wpuser
      - WORDPRESS_DB_NAME=wp
      - WORDPRESS_DB_PASSWORD=geheim

networks:
  wp_net:

volumes:
  wp_dbvolume:
  wp_htmlvolume:
```

Richtige Lösung
```dockerfile
version: "3.8" services:
wp_mariadb:
image: mariadb:latest volumes: - wp_dbvolume:/var/lib/mysql environment:
MYSQL_ROOT_PASSWORD: strenggeheim MYSQL_DATABASE: wp MYSQL_USER: wpuser MYSQL_PASSWORD: geheim networks: - wp_net
wp_pma: image: phpmyadmin/phpmyadmin environment:
PMA_HOST: wp_mariadb
ports: - "8080:80"
networks: - wp_net
wp_wordpress:
image: wordpress volumes: - wp_htmlvolume:/var/www/html/wp-content environment: WORDPRESS_DB_HOST: wp_mariadb WORDPRESS_DB_USER: wpuser WORDPRESS_DB_NAME: wp WORDPRESS_DB_PASSWORD: geheim
ports: - "8081:80"
networks: - wp_net
volumes:
wp_dbvolume: wp_htmlvolume:
networks: wp_net: ipam:
config: - subnet: 192.168.200.0/24
gateway: 192.168.200.1
```

```bash
docker-compose up -d
```

![image-20230522112338687](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230522112338687.png)

### Aufgabe 4.3-3

```bash
version: "3.8"

services:
  wp_mariadb:
    image: mariadb:latest
    volumes:
    - wp_dbvolume:/var/lib/mysql
    environment:
    - MYSQL_ROOT_PASSWORD_FILE=/run/secrets/mariadb_root_password
    - MYSQL_DATABASE=wp
    - MYSQL_USER=wpuser
    - MYSQL_PASSWORD_FILE=/run/secrets/mariadb_password
    networks:
    - wp_net

  wp_pma:
    image: phpmyadmin/phpmyadmin
    environment:
    - PMA_HOST=wp_mariadb
    ports:
    - "8080:80"
    networks:
    - wp_net

  wp_wordpress:
    image: wordpress
    volumes:
    - wp_htmlvolume:/var/www/html/wp-content
    environment:
    - WORDPRESS_DB_HOST=wp_mariadb
    - WORDPRESS_DB_USER=wpuser
    - WORDPRESS_DB_NAME=wp
    - WORDPRESS_DB_PASSWORD_FILE=/run/secrets/wordpress_db_password
    ports:
    - "8081:80"
    networks:
    - wp_net

volumes:
  wp_dbvolume:
  wp_htmlvolume:

networks:
  wp_net:
    ipam:
      config:
        - subnet: 192.168.200.0/24
          gateway: 192.168.200.1

secrets:
  mariadb_root_password:
    file: mariadb_root_password.txt
  mariadb_password:
    file: mariadb_password.txt
  wordpress_db_password:
    file: wordpress_db_password.txt
```

Dateien mit Passwort darin erstellen

```bash
docker-compose up -d
```

![image-20230522123306953](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230522123306953.png)



## Lernziele



**Sie können die Begriffe "Containerisierung", "Image", "Layer", "Container", "Repository", "Registry" und Dockerfile erklären**

1. Containerisierung: Containerisierung ermöglicht es, Anwendungen in isolierten Behältern (Containern) auszuführen, die alle benötigten Dateien und Einstellungen enthalten. Dies sorgt für Konsistenz und Portabilität der Anwendung.
2. Image: Ein Image ist eine Datei, die als Vorlage für die Erstellung von Containern dient. Es enthält alle benötigten Dateien und Konfigurationen, um eine Anwendung auszuführen.
3. Layer: Ein Layer ist eine einzelne Schicht innerhalb eines Docker-Images. Images bestehen aus mehreren Schichten, die Änderungen und Ergänzungen zum Dateisystem enthalten. Layer ermöglichen eine effiziente Speicherung und Verwaltung von Images.
4. Container: Ein Container ist eine isolierte Ausführungsumgebung für eine Anwendung. Es basiert auf einem Image und enthält alles, was benötigt wird, um die Anwendung auszuführen. Container sind konsistent und können auf verschiedenen Systemen ausgeführt werden.
5. Repository: Ein Repository ist ein Speicherort für Docker-Images. Es ermöglicht das Speichern, Verwalten und Teilen von Images. Ein Repository enthält verschiedene Versionen und Varianten eines Images.
6. Registry: Eine Registry ist ein Dienst, der als zentraler Speicherort für Docker-Images dient. Registries ermöglichen das Speichern, Verteilen und Verwalten von Images. Es gibt öffentliche Registries wie Docker Hub und private Registries für die interne Nutzung.
7. Dockerfile: Ein Dockerfile ist eine Textdatei, die Anweisungen enthält, um ein Docker-Image zu erstellen. Es beschreibt den Aufbau des Images, wie das Basisimage ausgewählt wird, Dateien kopiert werden und welche Einstellungen vorgenommen werden. Dockerfiles automatisieren die Image-Erstellung und ermöglichen eine einfache Wiederholbarkeit und Anpassung.



**Sie können die Begriffe Virtualisierung und Containerisierung voneinander trennen. **

Virtualisierung:

- Bei der Virtualisierung wird eine virtuelle Maschine (VM) erstellt, die als eigenständiger Computer fungiert und eine vollständige Betriebssysteminstanz mit allen benötigten Ressourcen enthält.
- Jede virtuelle Maschine emuliert eine physische Hardwareebene und kann verschiedene Betriebssysteme und Anwendungen ausführen.
- Virtualisierung ermöglicht die Isolierung von Anwendungen und bietet eine hohe Flexibilität, da verschiedene Betriebssysteme und Versionen parallel ausgeführt werden können.
- Es erfordert jedoch mehr Ressourcen, da jede virtuelle Maschine ihre eigenen Betriebssystemressourcen benötigt.

Containerisierung:

- Bei der Containerisierung werden Anwendungen in isolierten Containern ausgeführt, die eine leichte und schnelle Alternative zur Virtualisierung bieten.
- Containerisierung basiert auf der Nutzung des Host-Betriebssystems und des Kernel-Sharings, wodurch mehrere Container auf einem einzigen Host-System ausgeführt werden können.
- Container teilen sich den Host-Kernel, sind jedoch in ihrer Umgebung isoliert, sodass jede Anwendung ihre eigenen Dateisysteme, Prozesse und Netzwerkschnittstellen hat.
- Container sind leichtgewichtiger und starten schneller als virtuelle Maschinen. Sie bieten eine bessere Ressourceneffizienz und Skalierbarkeit.
- Containerisierung ist ideal für die Bereitstellung von Microservices und die schnelle Entwicklung und Bereitstellung von Anwendungen.

Zusammenfassend lässt sich sagen, dass Virtualisierung eine vollständige Virtualisierung von Hardware und Betriebssystemen ermöglicht, während Containerisierung auf der Isolierung von Anwendungen basiert und eine leichtgewichtige Alternative mit besserer Skalierbarkeit und Effizienz bietet.



**Sie kennen die elementaren Commands, um Images und Container zu erzeugen, starten, beenden und löschen**



**Sie können die wichtigsten Optionen von docker run (--name, --rm, --network, --ip, -d, -it, -p, -v, -e) korrekt anwenden**

- `--name <name>`: Gibt dem Container einen Namen, mit dem er leicht identifiziert werden kann. Zum Beispiel: `docker run --name my-container image-name:tag`.
- `--rm`: Löscht den Container automatisch, sobald er beendet wird. Dies ist praktisch, um temporäre Container zu erstellen, die nach der Ausführung nicht mehr benötigt werden. Zum Beispiel: `docker run --rm image-name:tag`.
- `--network <network>`: Weist den Container einem bestimmten Docker-Netzwerk zu. Dadurch können Container miteinander kommunizieren, indem sie den Netzwerknamen verwenden. Zum Beispiel: `docker run --network my-network image-name:tag`.
- `--ip <ip>`: Weist dem Container eine bestimmte IP-Adresse zu, wenn er mit einem benutzerdefinierten Netzwerk verbunden ist. Zum Beispiel: `docker run --network my-network --ip 192.168.0.10 image-name:tag`.
- `-d`: Startet den Container im Hintergrund (detached mode), sodass er im Hintergrund ausgeführt wird und die Kontrolle an die Shell zurückgegeben wird. Zum Beispiel: `docker run -d image-name:tag`.
- `-it`: Startet den Container im interaktiven Modus und bindet die Standardein- und -ausgabe an die Shell. Dies ermöglicht die interaktive Kommunikation mit dem Container. Zum Beispiel: `docker run -it image-name:tag`.
- `-p <host-port>:<container-port>`: Leitet den Verkehr vom Host-Port zum Container-Port weiter, wodurch die Verbindung zur Anwendung im Container hergestellt wird. Zum Beispiel: `docker run -p 8080:80 image-name:tag` leitet den Verkehr vom Host-Port 8080 zum Container-Port 80 weiter.
- `-v <host-path>:<container-path>`: Bindet einen Host-Verzeichnispfad an einen Pfad innerhalb des Containers. Dadurch können Daten zwischen dem Host und dem Container ausgetauscht werden. Zum Beispiel: `docker run -v /host/path:/container/path image-name:tag`.
- `-e <key=value>`: Setzt eine Umgebungsvariable im Container. Diese Option kann verwendet werden, um Konfigurationswerte an die Anwendung im Container zu übergeben. Zum Beispiel: `docker run -e ENV_VARIABLE=value image-name:tag`.



**Sie können verschiedene Versionen (tags) eines Container-Images nutzen**

```bash
docker run my-image:v1.0   // Startet den Container mit Tag v1.0
docker run my-image:v1.1   // Startet den Container mit Tag v1.1
docker run my-image:latest // Startet den Container mit dem als latest markierten Tag
```

Standardmässig nimmt es **latest**.



**Sie können Portweiterleitungen in Betrieb nehmen**

hostport:containerport

Weitere Infos oben.



**Sie können benannte und gemountete Volumen korrekt einsetzen**

Volumename:Containerverzeichnis

Hostverzeichnis:Containerverzeichnis

Weitere Infos oben.



**Sie verstehen den Standardaufbau eines Netzwerks mit Docker**

Weitere Infos oben.



**Sie können Docker-Netzwerke definieren und diese den Containern zuweisen**

Weitere Infos oben.



**Sie kennen die Syntax von Dockerfiles**

1. - `FROM`: Gibt das Basisimage an, auf dem das neue Image aufgebaut wird.
   - `RUN`: Führt einen Befehl innerhalb des Images aus, um Pakete zu installieren, Abhängigkeiten zu konfigurieren usw.
   - `COPY` oder `ADD`: Kopiert Dateien oder Verzeichnisse vom Host in das Image.
   - `WORKDIR`: Setzt das Arbeitsverzeichnis für nachfolgende Anweisungen innerhalb des Containers.
   - `EXPOSE`: Deklariert die Ports, auf denen der Container Anwendungen verfügbar macht.
   - `CMD` oder `ENTRYPOINT`: Gibt den Befehl an, der beim Starten des Containers ausgeführt werden soll.
2. Variablen: Um die Lesbarkeit und Wiederverwendbarkeit des Dockerfiles zu verbessern, können Variablen verwendet werden. Sie werden mit dem Format `$VARIABLE_NAME` oder `${VARIABLE_NAME}` dargestellt und können entweder im Dockerfile selbst oder über Umgebungsvariablen gesetzt werden.
3. Schichtenaufbau: Dockerfiles verwenden eine schichtbasierte Struktur. Jede Anweisung in einem Dockerfile erzeugt eine neue Schicht im Image. Schichten sind effizient, da sie wiederverwendet werden können, wenn sich die vorherigen Schichten nicht ändern.

Beispiel:

```dockerfile
# Kommentar: Dockerfile für meine Anwendung

# Setzen des Basisimages
FROM ubuntu:latest

# Ausführen von Anweisungen innerhalb des Images
RUN apt-get update && apt-get install -y \
    package1 \
    package2

# Kopieren von Dateien vom Host in das Image
COPY app /app

# Festlegen des Arbeitsverzeichnisses
WORKDIR /app

# Exponieren eines Ports
EXPOSE 8080

# Befehl, der beim Starten des Containers ausgeführt wird
CMD ["python", "app.py"]
```

**Sie können die 12 verschiedenen Anweisungen von Dockerfiles erklären**



**Sie können zwischen ENTRYPOINT und CMD sowie COPY und ADD unterscheiden** 

`ENTRYPOINT` und `CMD` unterscheiden sich in ihrer Verwendung und ihrem Verhalten:

- `ENTRYPOINT`: Definiert den Befehl oder das Skript, das immer ausgeführt wird, wenn der Container gestartet wird. Es stellt den Hauptbefehl des Containers dar und kann nicht überschrieben werden. Wenn `ENTRYPOINT` zusammen mit `CMD` verwendet wird, fungiert `CMD` als Argumente für `ENTRYPOINT`.
- `CMD`: Gibt den Standardbefehl oder das Standardskript an, das beim Starten des Containers ausgeführt wird. Es kann überschrieben werden, indem beim Starten des Containers Befehle oder Argumente angegeben werden. Wenn `CMD` zusammen mit `ENTRYPOINT` verwendet wird, stellt `CMD` Argumente für den Befehl dar, der durch `ENTRYPOINT` angegeben ist.

Beispiel

```dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo", "Hello"]
CMD ["world"]
```



- `COPY` und `ADD` werden verwendet, um Dateien oder Verzeichnisse vom Host in das Image zu kopieren, haben aber einige Unterschiede:

- `COPY`: Kopiert Dateien oder Verzeichnisse vom Host in das Image. Es unterstützt grundlegende Kopierfunktionen und ist die bevorzugte Option, wenn Sie einfach Dateien in das Image kopieren möchten.

- `ADD`: Ähnlich wie `COPY`, kann aber auch URLs oder TAR-Archive verarbeiten. Es hat zusätzliche Funktionen wie das Entpacken von TAR-Archiven und das Herunterladen von Inhalten von URLs. Es wird empfohlen, `ADD` nur zu verwenden, wenn Sie die spezifischen Funktionen benötigen.

  Im Allgemeinen wird empfohlen, `COPY` für einfache Kopieroperationen zu verwenden und `ADD` nur dann einzusetzen, wenn die zusätzlichen Funktionen benötigt werden.



## Miniprojekt

1. Ein Lokales Verzeichnis **webserver** erstellen und auf das Verzeichnis werchseln.

```bash
mkdir webserver
cd webserver
```

![image-20230519114214782](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230519114214782.png)

2. Dockerfile und Dateistruktur erstellen

```bash
nano Dockerfile
```

Konfigurationen vornehmen

```doc
FROM nginx

WORKDIR /usr/share/nginx/html

COPY ./public/ /usr/share/nginx/html

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
```

**From** bestimmt das Basisimage

**Workdir** setzt das Arbeitsverzeichnis

**Copy** kopiert Dateien aus dem Hostsystem ins Image

**Expose** öffnet den angegebenen Port

Der letzte Befehl ist dafür verantwortlich, dass sich Nginx beim Containerstart automatisch startet

Dateistruktur für den **Copy** Befehl:

![image-20230519121107102](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230519121107102.png)

3. Image bauen

```bash
docker build -t mein-webserver .
```

-t steht für **Tag**, also zur Identifizierung des Images.

Der Punkt am Ende bedeutet einfach, dass sich das Dockerfile im aktuellen Verzeichnis befindet.

![image-20230519121343832](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230519121343832.png)

4. Container starten

```bash
docker run -p 8080:80 -v /var/log:/var/log/nginx --name mein-container mein-webserver
```

![image-20230519123753121](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230519123753121.png)

![image-20230519123832430](C:\Users\incre\AppData\Roaming\Typora\typora-user-images\image-20230519123832430.png)



## Microservices, Entwicklung mit dotnet



### Aufgabe 7.4-1

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /build
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:6.0
LABEL description="Minimal Api with MongoDB"
LABEL organisation="KSB St. Gallen"
LABEL author="Manuel Bruelisauer"
WORKDIR /app
COPY --from=build-env /build/out .
ENTRYPOINT ["dotnet", "app.dll"]
```

```bash
docker run -p 5001:80 myminimalapi
```

Dieser Command war wichtig, da der Port auf dem Localhost sonst nicht richtig funktioniert hat.



Docker compose file:
```bash
version: "3.9"
services:
  myminimalapi:
    build: WebApi
    ports:
      - 5001:80
```



### Aufgabe 7.4-2

Um Applikation auszuführen

```bash
dotnet run
```

```bash
code .
```

```bash
docker run -d --name my-mongodb -p 27017:27017 -v mydata:/data/db mongo
```

```bash
dotnet add package MongoDB.Driver
```

```c#
using MongoDB.Driver;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Minimal API Version 1.0");

// app.MapGet("/check", () => { /* Code zur Prüfung der DB ...*/ return "Zugriff auf MongDB ok.";});

app.MapGet("/check", () =>
{
    try
    {
        var mongoDbConnectionString = "mongodb://localhost:27017";
        var mongoClient = new MongoClient(mongoDbConnectionString);
        var databaseNames = mongoClient.ListDatabaseNames().ToList();
        
        return "Zugriff auf MongoDB ok. Vorhandene DBs: " + string.Join(",", databaseNames);
    }
    catch (System.Exception e)
    {
        return "Zugriff auf MongoDB funktioniert nicht: " + e.Message;
    }
});


app.Run();
```

Neues cs:

```c#
using MongoDB.Driver;

var builder = WebApplication.CreateBuilder(args);

var movieDatabaseConfigSection = builder.Configuration.GetSection("DatabaseSettings");
builder.Services.Configure<DatabaseSettings>(movieDatabaseConfigSection);

var app = builder.Build();

app.MapGet("/", () => "Minimal API Version 1.0");

// app.MapGet("/check", () => { /* Code zur Prüfung der DB ...*/ return "Zugriff auf MongDB ok.";});

app.MapGet("/check", (Microsoft.Extensions.Options.IOptions<DatabaseSettings> options) => 
{
    try
    {
        var mongoDbConnectionString = options.Value.ConnectionString;
        var mongoClient = new MongoClient(mongoDbConnectionString);
        var databaseNames = mongoClient.ListDatabaseNames().ToList();
        
        return "Zugriff auf MongoDB ok. Vorhandene DBs: " + string.Join(",", databaseNames);
    }
    catch (System.Exception e)
    {
        return "Zugriff auf MongoDB funktioniert nicht: " + e.Message;
    }
});


app.Run();
```

appsettings.json:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "DatabaseSettings": { "ConnectionString": "mongodb://localhost:27017"}
}
```

neues Docker-compose file:

```bash
version: "3.9"
services:
  mongodb:
    image: mongo

  myminimalapi:
    build: WebApi
    ports:
      - 5001:80
    depends_on:
      - mongodb
    environment:
      - MoviesDatabaseSettings__ConnectionString=mongodb://gbs:geheim@mongodb:27017
```



## Lernziele 2

**WebApi Projekt**

**Sie können mit [YAML-Dateien](https://moodle.gbssg.ch/mod/page/view.php?id=872) korrekt umgehen**

- Eine Liste wird mit `-` + Abstand gebildet, z.B.

```
# A list of tasty fruits
- Apple
- Orange
- Strawberry
- Mango
```

- Key-Value-Paare werden mit `:` + Abstand gebildet, z.B.:

  ```yaml
  name: Hans Martin
  ```

- 

  Objekte bestehend aus mehreren Key-Value-Paaren, können durch Zeilenumbrüche gebildet werden

  ```yaml
  vorname: Hans Martin
  nachname: Keller
  alter: 20
  ```

- 

  Ein Value kann seinerseits wiederum rekursiv aus Objekten, Key-Value-Paaren oder Listen bestehen:

  ```yaml
  # Employee records
  - martin:
      name: Martin D'vloper
      job: Developer
      skills:
        - python
        - perl
        - pascal
  - tabitha:
      name: Tabitha Bitumen
      job: Developer
      skills:
        - lisp
        - fortran
        - erlang              
  ```

- Die Strukturierung der Elemente wird durch Einrückungen erreicht. Dabei **müssen Leerzeichen** verwendet werden. Tabulatoren auch am Zeilenende führen zu Fehlern.



**Sie kennen die Syntax von docker-compose.yml und können die Schlüsselwörter korrekt anwenden**

- **version**: zur Zeit "3.8"
- **services**: Zur Definition der einzelnen Container. Die Dienstnamen (Keys) für die Container können freigewählt werden.
- **networks**: Zur Definition eigener Netzwerke
- **volumes**: Angabe von benannten Volumes
- **secrets**: Angabe von Passwortdateien

```yaml
# Grundsätzlicher Aufbau von docker-compose.yml
version: "3.8"
services:
  dienstname1:
    schlüsselwort1: einstellung1
    schlüsselwort2: einstellung2
    ...
  dienstname2:
    schlüsselwort1: einstellung1
    schlüsselwort2: einstellung2
    ...
volumes:
networks:
secrets:
```

Die wichtigsten Schlüsselwörter unterhalb der Dienstnamen sind:

- **image**: Das Basisimage eines Service oder
- **build**: Ein Verzeichnis mit Dockerfile, der Service wird dann nicht aus einem Image gestartet sondern aus einem Dockerfile
- **container_name**: Der Name für den resultierenden Container
- **restart**: always, on-failure oder unless-stopped
- **environment**: Liste mit Umgebungsvariablen für einen Container
- **volumes**: Liste der gemounteten Volumes
- **ports**: Liste der Portweiterleitungen
- **expose**: Ports für die Kommunikation zwischen Containern
- **networks**: Verweis auf ein im Top-Level-Schlüsselwort networks definiertes Netzwerk
- **secrets**: Verweis auf die im Top-Level-Schlüsselwort secrets definierte Passwortdatei

Hier nun die Schlüsselwörter im Detail:

**image**

Wenn das Basisimage in Dockerhub verfügbar ist, kann es direkt verwendet werden:

```yaml
services: 
  my-service:
    image: ubuntu:latest
    ...
```

**build**

Wird ein eigenes Image benötigt, kann dieses aus dem angegebenen Dockerfile erstellt werden:

```yaml
services: 
  my-custom-app:
    build: /path/to/dockerfile/
    ...
```

**container_name**

Der Name für den resultierenden Container bei docker ps (analog --name bei docker run)

```yaml
services: 
  my-custom-app:
    container_name: my-container
    ...
```

Wird der container_name nicht angegeben, resultiert für den Namen des Containers eine Zusammensetzung aus Arbeitsverzeichnis, Servicename und einer Laufnummer, z.B. workdir-my-custom-app-1. Der Name des Services entspricht also **nicht** dem Namen des Containers.

**restart**

Dieser Eintrag definiert die Restart-Policy für einen Container. Folgende Werte sind möglich

- **no**: Der Standard, Container wird nicht automatisch gestartet
- **on-failure**: Der Container wird bei einem Fehler (Exit-Code ungleich 0) automatich neu gestartet
- **always**: Der Container wird immer neu gestartet, insbesondere bei einem Reboot. Wenn der Container manuell gestopped wird, startet er neu, wenn der Dockerdaemon neu gestartet wird.
- **unless-stopped**: Ähnlich wie always, wird der Container manuell gestoppt, startet er nach einem Neustart des Dockerdaemons oder einem Reboot nicht mehr neu.

```yaml
services: 
  my-custom-app:
    restart: always
    ...
```

**ports**

Um einen Service vom Host aus zu erreichen, wird die Angabe der Portweiterleitungen benötigt:

```yaml
services:
  my-custom-app:
    image: myapp:latest
    ports:
      - "8080:3000"
      - "8081:4000"
    ...
```

**expose**

Angabe der Ports, welche die Container für die Kommunikation untereinander benötigen. Wenn ein Basisimage bereits einen Port exponiert, ist diese Angabe nicht nötig.

```yaml
services:
  network-example-service:
    image: karthequian/helloworld:latest
    expose:
      - "80"
```

**networks**

Hier kann ein unter dem Top-Level-Schlüsselwort definiertes Netzwerk referenziert werden.

```yaml
services:
  network-example-service:
    image: karthequian/helloworld:latest
    networks: 
      - my_network
    ...
  another-service-in-the-same-network:
    image: alpine:latest
    networks: 
      - my_network
    ...

networks:
  my_network: 
    ipam:
      config:
        - subnet: 172.17.0.0/24
          gateway: 172.17.0.1
```

In der Regel werden bei der Definition des Netzwerkes keine weiteren Optionen angegeben. Docker kümmert sich dann selber um die konkrete Definition:

```yaml
services:
  network-example-service:
    image: karthequian/helloworld:latest
    networks: 
      - my_network
    ...

networks:
  my_network: 
```

**volumes**

Gibt an wie Containervolumes auf den Host gemountet werden. Benannte Volumes müssen im Top-Level-Schlüsselwort volumes angegeben werden. Somit können benannte Volumes von mehreren Containern gleichzeitig verwendet werden. Der Zusatz :ro mountet ein Verzeichnis read-only.

```yaml
services:
  volumes-example-service:
    image: alpine:latest
    volumes: 
      - my-named-global-volume:/my-volumes/named-global-volume
      - /tmp:/my-volumes/host-volume
      - /home:/my-volumes/readonly-host-volume:ro
    ...
  another-volumes-example-service:
    image: alpine:latest
    volumes:
      - my-named-global-volume:/another-path/the-same-named-global-volume
    ...
volumes:
  my-named-global-volume:
```

Unbenannte Volumes und Volumes in eigene Verzeichnisse müssen nicht im Top-Level volume aufgeführt werden.
Es können auch einzelen Dateien gemountet werden.

**depends_on**

Um Abhängigkeiten zu definieren, kann depends_on verwendet werden. Die betreffenden Container werden dann zuerst geladen:

```yaml
services:
  kafka:
    image: wurstmeister/kafka:2.11-0.11.0.3
    depends_on:
      - zookeeper
    ...
  zookeeper:
    image: wurstmeister/zookeeper
    ...
```

**environment**

Variablen können sowohl statisch als auch dynamisch mit ${} definiert werden

```yaml
services:
  database: 
    image: "postgres:${POSTGRES_VERSION}"
    environment:
      DB: mydb
      USER: "${USER}"
```

Die dynamischen Variablen können dabei u.a. in einer Datei .env im selben Verzeihnis als Key-Value-Paare definiert werden:

```yaml
POSTGRES_VERSION=alpine
USER=foo
```

Die verfügbaren Umgebungsvariablen müssen in der Dokumentation zu einem Image auf Dockerhub nachgeschaut werden

**secrets**

Sollen Passwörter aus sicherheitstechnischen Gründen nicht in der docker-compose Datei aufgeführt werden, kann man secrets verwenden. Das Top-Level-Schlüsselwort gibt an, wo sich die Passwortdatei befindet. Die Einträge bei den Services referenzieren dann die Top-Level-Definition

```yaml
services:
   db:
     image: mysql:latest
     environment:
       MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
       MYSQL_PASSWORD_FILE: /run/secrets/db_password
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
     secrets:
       - db_root_password
       - db_password
     ...
secrets:
   db_password:
     file: db_password.txt
   db_root_password:
     file: db_root_password.txt
```

Die in MYSQL_ROOT_PASSWORD_FILE und MYSQL_PASSWORD_FILE angegebenen Dateien innerhalb des Containers enthalten dann die in den unter dem Top-Level-Schlüsselwort secrets angegebenen Passwörter. Die verfügbaren Secretsvariablen müssen in der Dokumentation zu einem Image auf Dockerhub nachgeschaut werden.



**Sie können das Kommando docker compose up/down mit den Schaltern -d und --build korrekt anwenden**

- `-d` wird verwendet, um die Container im Hintergrund (detach mode) auszuführen. Dadurch werden die Logs nicht auf dem Terminal ausgegeben.
- `--build` wird verwendet, um die Images der Dienste neu zu erstellen, falls Änderungen an den Dockerfile- oder Build-Kontextdateien vorgenommen wurden.

Beispiel:

- `docker-compose up -d`: Startet die Dienste im Hintergrund, ohne die Images neu zu erstellen.
- `docker-compose up --build`: Startet die Dienste und erstellt die Images neu, falls erforderlich.
- `docker-compose up -d --build`: Startet die Dienste im Hintergrund und erstellt die Images neu, falls erforderlich.

Das Kommando `docker-compose down` wird verwendet, um die Dienste in einem Docker-Compose-Projekt zu stoppen und die zugehörigen Container, Netzwerke und Volumes zu entfernen.



**Sie können Dockerfiles für Multistage Builds im Zusammenhang mit dotnet aufbauen**

**Sie können mit dotnet Web- und Konsolen-Anwendungen korrekt umgehen**
