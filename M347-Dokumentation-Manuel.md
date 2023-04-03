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