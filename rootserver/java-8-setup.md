# Setup Java 8

{% hint style="info" %}
Java 8 wird nur noch für sehr alte Anwendungen (z.B. Minecraft bis 1.16) benötigt. Für neuere Software solltet ihr [Java 17](java-17-setup.md) oder [Java 21](java-21-setup.md) verwenden.
{% endhint %}

AdoptOpenJDK wurde 2021 zu **Eclipse Temurin** (Adoptium) umbenannt. Das alte `adoptopenjdk.jfrog.io`-Repository wird nicht mehr gepflegt – stattdessen verwenden wir das aktuelle Adoptium-Repository.

* Aktualisiere zuerst die Paketlisten und installiere die benötigten Pakete.
``` bash
apt update && apt install wget gnupg ca-certificates -y
```

* Hinterlege den GPG-Schlüssel von Adoptium.
``` bash
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor -o /usr/share/keyrings/adoptium.gpg
```

* Füge das Adoptium-Repository hinzu.
``` bash
echo "deb [signed-by=/usr/share/keyrings/adoptium.gpg] https://packages.adoptium.net/artifactory/deb $(. /etc/os-release && echo "$VERSION_CODENAME") main" > /etc/apt/sources.list.d/adoptium.list
```

* Aktualisiere die Paketlisten und installiere Java 8 (Temurin).
``` bash
apt update && apt install temurin-8-jdk -y
```

Mit diesem Befehl kannst du die Version überprüfen

``` bash
java -version
```
Beispielausgabe:
```
openjdk version "1.8.0_432"
OpenJDK Runtime Environment (Temurin)(build 1.8.0_432-b06)
OpenJDK 64-Bit Server VM (Temurin)(build 25.432-b06, mixed mode)
```
