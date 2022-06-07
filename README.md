# Einleitung allgemein
Einleitung allgemein (Erklärungen zum ganzen M300-Projekt)

# Inhaltsverszeichnis
- [Einleitung allgemein](#einleitung-allgemein)
- [Inhaltsverszeichnis](#inhaltsverszeichnis)
  - [Vagrant](#vagrant)
    - [Apache Webserver automatisiert aufsetzen](#apache-webserver-automatisiert-aufsetzen)
  - [Docker](#docker)
    - [Docker erklärt](#docker-erklärt)
    - [Befehle](#befehle)
    - [Datenbank mit Docker aufsetzen](#datenbank-mit-docker-aufsetzen)
    - [Persistente Daten](#persistente-daten)
  - [20-Infrastruktur](#20-infrastruktur)
  - [35-Sicherheit 1](#35-sicherheit-1)
  - [30-Container](#30-container)
  - [35-Sicherheit 2](#35-sicherheit-2)
  - [40-Container-Orchestrierung](#40-container-orchestrierung)
  - [50-Add-ons](#50-add-ons)
  - [60-Reflexion](#60-reflexion)
  - [Quellen](#quellen)


## Vagrant
Vagrant ist ein deklarativer Deployment anbieter für Containerumgebungen. Es ermöglicht uns, anhand eines Scriptes, ein System aufzusetzen. Dadurch, dass man die VM's jeweils immer vom gleichen Script deployed, sind sie konsistent und ersparen viel Arbeit. Wenn Probleme mit dem Server auftauchen, kann die VM einfach "Destroyed" werden und neu aufgesetzt, und schon ist die VM wieder clean.<br>
Im Vagrantfile gibt man das Image an, welches man deployen möchte (z.B. Ubuntu) und verlinkt auch ein Provisioning file, in dem man zusätzliche Befehle für das System hinzufügen kann. Es lohnt sich z.B. in diesem Provisioning file den Befehl <b>sudo apt-get update</b> einzubauen, dass die VM jeweils direkt geupdated wird. Natürlich kann man auch gleich Installationen durchführen, wie z.B. <b>sudo apt install nginx</b>.

        sudo apt-get update
        sudo apt-get install nginx

### Apache Webserver automatisiert aufsetzen
Mit Vagrant kann man automatisiert ein System anhand von einem yml script erstellen. Das ist das Deklarative Deployment, also es wird nicht jede Maschine manuell deployed. Wir deployen hier einen Apache 2 Webserver anhand des Vagrant files.

Nachfolgend wird die VM mit einem bereits abgeänderten File bzw. VM aus dem M300-Repository erstellt:

1. Terminal öffnen
2. In das M300-Verzeichnis (/home/ubuntu/M300/vagrant/web) wechseln:

        cd /home/ubuntu/M300/vagrant/web

3. VM erstellen und starten:

        vagrant up

4. Webbrowser auf dem Laptop starten und über die IP (10.4.43.12:8080 verbinden.)
5. Im Ordner /web die Hauptseite index.html editieren bzw. durch eine andere ersetzen (z.B. HTML5up-Themplate) und das Resultat überprüfen
6. Abschliessend kann die VM wieder gelöscht werden:
   
        vagrant destroy -f
7. Vagrant ist nun komplett einsatzfähig!

## Docker
### Docker erklärt
Docker ist ein Container anbieter, welcher uns ermöglicht, anhand vom Docker Deamon, unsere Container zu deployen und zu managen. Es besteht aus verschiedenen Komponenten, welche uns dabei unterstützen:<br>
<img src="ImagesDocs/docker_aufstellung.png" alt="Docker Übersicht" width="350"><br>

<b>Daemon</b><br>
Der Docker Daemon ist für das Managen der Infrastruktur zuständig. Er weiss, wo die Images abgelegt werden, welche Container gerade laufen und welches die standard registry ist. Wir kommunizieren also über den Daemon mit den Docker-containern.<br>

<b>Images</b><br>
Docker basiert auf Images. Dort drin sind alle benötigten Informationen vorhanden, um dann einen Container zu erstellen. Sie sind also sozusagen Containervorlagen, welche man zu containern ausführen kann. Diese Images müssen aber zuerst lokal in der Image-Umgebung platziert werden. Es stehen uns externe Registries zur Verfügung, wie z.B. Dockerhub, von dem wir vorgefertigte images direkt herunterladen können. Es ist auch möglich, ein image selbst zu erstellen und von diesem zu deployen.<br>
Wie bereits gesagt, nimmt der Deamon dann ein Image und deployed es als Container.

### Befehle
Dies ist ein Cheat sheet zu Docker Containern, auf dem man alle wichtigen Befehle sieht:
[docker_cheat_sheet.pdf](ImagesDocs/docker_cheat_sheet.pdf)

### Datenbank mit Docker aufsetzen
Wir benutzen die TBZ-Maas-VM von Herrn Calisto, welche eine Ubuntu Distribution am laufen hat. Darauf installieren wir erst mal Docker.

    sudo apt-get install docker

Im nächsten Schritt suchen wir uns ein passendes Docker-image auf dem Dockerhub:
<img src="ImagesDocs/docker_hub_mariadb.png" alt="Docker MariaDB" width="350"><br>

Wir laden das ubuntu/mysql image vom Dockerhub herunter:

    docker pull ubuntu/mysql

Mit docker images kann man alle images anzeigen auf dem System. Dann starten wir den Container mit dem docker run Befehl:

    docker images
    docker run --name <containername> -e MYSQL_ROOT_PASSWORD=<password> -p 3306:3306 -d ubuntu/mysql

--name ist der Name, den wir für den Container definieren. Dann geben wir ein Passwort an und mit -p führen wir eine Portweiterleitung vom Container zum Host durch. Diese ist nötig, damit man vom Host auf die Datenbank zugreifen kann.<br>
Mit einem Datenbankclient wie z.B. MySQL Workbench können wir jetzt auf die Datenbank connecten.

### Persistente Daten
Damit unsere Daten konsistent bleiben im Container (also wenn der Container gelöscht und wiederaufgebaut wird bleibt alles gleich) konsistent bleiben, benötigen wir einen verlinkten Ordner. Um das zu erreichen, mounten wir zwei Ordner miteinander. Im Fall von mysql ist das /var/lib/mysql mit irgend einem Ordner auf unserem lokalen Rechner, wie z.B. /home/lian/mysql.<br>

Um diese Verlinkung der Volumes zu bekommen, müssen wir erstmals ein Volume erstellen:

    docker volume create mydbstore

Nun können wir überprüfen, mit welchem lokalen Ordner dieses Volume verknüpft ist, indem wir es inspizieren:

    docker volume inspect mydbstore

Unter "Mountpoint" sehen wir den lokalen Pfad des verlinkten volumes. Nun passen wir unseren Docker run Befehl nochmals an. Wir fügen <b>-v mydbstore:/var/lib/mysql</b> hinzu, das bedeutet, dass wir dieses erstellte Volume gerne verknüpfen möchten mit dem Order /var/lib/mysql.

    docker run --name <containername> -e MYSQL_ROOT_PASSWORD=<password> -v mydbstore:/var/lib/mysql -p 3306:3306 -d ubuntu/mysql

Nun kann man wieder auf die DB connecten und dort einen neuen table erstellen. Dieser wird direkt auf unseren lokal gemounteten Ordner kopiert und wenn wir die DB löschen und wieder starten, werden die Daten erfolgreich wiederhergestellt.

## 20-Infrastruktur
Einträge (eigene Erkenntnisse während dem Bearbeiten dieses Kapitels)

## 35-Sicherheit 1
Einträge (eigene Erkenntnisse während dem Bearbeiten dieses Kapitels)

## 30-Container
Einträge (eigene Erkenntnisse während dem Bearbeiten dieses Kapitels)

## 35-Sicherheit 2
Einträge (eigene Erkenntnisse während dem Bearbeiten dieses Kapitels)

## 40-Container-Orchestrierung
Einträge (eigene Erkenntnisse während dem Bearbeiten dieses Kapitels)

## 50-Add-ons 
Einträge (eigene Erkenntnisse während dem Bearbeiten dieses Kapitels)

## 60-Reflexion
Lernprozess festgehalten (Form frei wählbar)

## Quellen
- https://github.com/mc-b/M300/
- https://web.microsoftstream.com/video/df4e25df-b998-4c19-9263-17d3f32a6015?list=user&userId=91407230-9462-4d68-a2e7-13e41cfe3aaf&referrer=https:%2F%2Fwww.google.com%2F

- - -
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/ch/"><img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/ch/88x31.png" /></a><br />Dieses Werk ist lizenziert unter einer <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/ch/">Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 3.0 Schweiz Lizenz</a>

- - -
