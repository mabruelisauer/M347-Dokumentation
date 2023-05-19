# Miniprojekt



## Schritte

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