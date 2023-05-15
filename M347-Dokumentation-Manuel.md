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



### Miniprojekt

So guet wie fertig (Verlauf)
