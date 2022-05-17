# M300
## Apache Webserver automatisiert aufsetzen
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