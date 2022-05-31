# Einleitung allgemein
Einleitung allgemein (Erklärungen zum ganzen M300-Projekt)

# Inhaltsverszeichnis
hallo

## 10-Toolumgebungen 

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

## Quellen
- https://github.com/mc-b/M300/

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


- - -
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/ch/"><img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/ch/88x31.png" /></a><br />Dieses Werk ist lizenziert unter einer <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/ch/">Creative Commons Namensnennung - Nicht-kommerziell - Weitergabe unter gleichen Bedingungen 3.0 Schweiz Lizenz</a>

- - -
