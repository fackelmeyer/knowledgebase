# Setup Java 21

Java 21 ist die aktuelle LTS-Version und wird für **Minecraft 1.20.5 und neuer** (u.a. 1.21.x) benötigt. Für ältere Minecraft-Versionen reicht [Java 17](java-17-setup.md).

## Installation aus den Distributionspaketen

`openjdk-21` ist in Debian 13 (trixie) sowie Ubuntu 24.04 enthalten.

* Aktualisiere zuerst deine Paketlisten und installiere mögliche Updates.
``` bash
apt update && apt upgrade -y
```

* Installiere Java 21.
``` bash
apt install openjdk-21-jdk openjdk-21-jre -y
```

Mit diesem Befehl kannst du die Version überprüfen

``` bash
java -version
```
Beispielausgabe:
```
openjdk version "21.0.5" 2024-10-15
OpenJDK Runtime Environment (build 21.0.5+11-Debian-1)
OpenJDK 64-Bit Server VM (build 21.0.5+11-Debian-1, mixed mode, sharing)
```

## Installation über Eclipse Temurin (empfohlen für Debian 12)

In Debian 12 (bookworm) ist `openjdk-21` standardmäßig nicht enthalten. Verwende dort das offizielle Adoptium-Repository – dieses funktioniert auf allen Debian- und Ubuntu-Versionen.

* Hinterlege den GPG-Schlüssel und das Repository von Adoptium.
``` bash
apt install wget gnupg ca-certificates -y
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor -o /usr/share/keyrings/adoptium.gpg
echo "deb [signed-by=/usr/share/keyrings/adoptium.gpg] https://packages.adoptium.net/artifactory/deb $(. /etc/os-release && echo "$VERSION_CODENAME") main" > /etc/apt/sources.list.d/adoptium.list
```

* Aktualisiere die Paketlisten und installiere Temurin 21.
``` bash
apt update && apt install temurin-21-jdk -y
```
