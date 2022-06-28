# Einleitung
Das LB2 Projekt wurde etwas verkürzt, da noch die TOB Projektwoche dazwischen kam. Das Ziel ist es, einen Webserver aufzusetzen als Frontend, und einen MySQl-Container als Backend. Diese werden dann miteinander verknüpft.

# Inhaltsverszeichnis
- [Einleitung](#einleitung)
- [Inhaltsverszeichnis](#inhaltsverszeichnis)
  - [Umsetzung](#umsetzung)
  - [Testing](#testing)
  - [Quellen](#quellen)


## Umsetzung
Zuerst installieren wir Docker Compose, falls es noch nicht installiert ist.

        sudo apt-get install docker-compose
        sudo apt-get install docker

Dann erstellen / installieren wir die images vom Docker hub.
        docker pull httpd
        docker pull mysql


## Testing
Text

## Quellen
https://github.com/mc-b/M300/tree/master/docker/compose