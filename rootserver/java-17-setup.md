# Setup Java 17

Java 17 (LTS) wird für Minecraft 1.18 bis 1.20.4 benötigt (und läuft auch mit 1.17). Für Minecraft 1.20.5 und neuer ist [Java 21](java-21-setup.md) erforderlich.

{% hint style="info" %}
Die früher hier beschriebene Installation über `oracle-java17-installer` bzw. das `linuxuprising`-PPA wird nicht mehr gepflegt und funktioniert auf aktuellen Systemen nicht mehr. Auf Debian und Ubuntu installiert ihr Java 17 am einfachsten direkt aus den offiziellen Paketquellen.
{% endhint %}

* Aktualisiere zuerst deine Paketlisten und installiere mögliche Updates.
``` bash
apt update && apt upgrade -y
```

* Installiere Java 17 aus den Distributionspaketen.
``` bash
apt install openjdk-17-jdk openjdk-17-jre -y
```

Mit diesem Befehl kannst du die Version überprüfen

``` bash
java -version
```
Beispielausgabe:
```
openjdk version "17.0.13" 2024-10-15
OpenJDK Runtime Environment (build 17.0.13+11-Debian-2deb12u1)
OpenJDK 64-Bit Server VM (build 17.0.13+11-Debian-2deb12u1, mixed mode, sharing)
```

{% hint style="warning" %}
`openjdk-17` ist in Debian 11/12 und Ubuntu 22.04/24.04 enthalten. Sollte das Paket in deiner Distribution nicht verfügbar sein, kannst du stattdessen **Eclipse Temurin** verwenden.
{% endhint %}

* Hinterlege den GPG-Schlüssel und das Repository von Adoptium.
``` bash
apt install wget gnupg ca-certificates -y
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor -o /usr/share/keyrings/adoptium.gpg
echo "deb [signed-by=/usr/share/keyrings/adoptium.gpg] https://packages.adoptium.net/artifactory/deb $(. /etc/os-release && echo "$VERSION_CODENAME") main" > /etc/apt/sources.list.d/adoptium.list
```

* Installiere anschließend Temurin 17.
``` bash
apt update && apt install temurin-17-jdk -y
```
