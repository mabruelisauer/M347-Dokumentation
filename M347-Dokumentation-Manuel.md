# M347 Dokumentation



## Grundlagen

**Containerisierung** ist sozusagen die Virtualisierung mit Hilfe von **Containern**. Die einzelnen Container sind laufende Rechenumgebungen.

​					![VM vs containers](https://cdn.hashnode.com/res/hashnode/image/upload/v1596298477648/alMXfcmFx.png?auto=compress,format&format=webp)



![Virtuelle Maschinen vs. Container](https://gbssg.gitlab.io/m347/img/kap1/1-1.PNG)



Hauptunterschied ist also, dass Container weniger Ressourcen benötigen. Dies ist so da bei Containern nicht ganze Betriebssysteme betrieben werden müssen. Ebenfalls sind Container auf jedem Betriebssystem verfügbar. Docker engine übernimmt die Verteilung der Ressourcen.



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
