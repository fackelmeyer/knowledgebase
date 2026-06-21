# Docker Installation

In dieser Anleitung installierst du Docker Engine inklusive Docker Compose (v2) über das offizielle Docker-Repository. Diese Methode ist sowohl für Debian (11/12/13) als auch für Ubuntu (22.04/24.04) aktuell.

* Aktualisiere die Paketlisten & installiere die Updates.
```bash
apt update && apt upgrade -y
```

{% hint style="warning" %}
Achte auf dein Betriebssystem.
Solltest du nicht wissen, welches Betriebssystem du verwendest, kannst du dies mit dem Befehl nachschauen.
{% endhint %}
```bash
cat /etc/issue
```

* Installiere die benötigten Pakete für die Einrichtung des Repositories.
```bash
apt install ca-certificates curl -y
```

* Lege das Verzeichnis für den GPG-Schlüssel an.
```bash
install -m 0755 -d /etc/apt/keyrings
```

{% tabs %}
{% tab title="Debian" %}
* Lade den offiziellen GPG-Schlüssel von Docker herunter.

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc
```

* Füge das Docker-Repository zu deinen Paketquellen hinzu.

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
```

{% endtab %}

{% tab title="Ubuntu" %}
* Lade den offiziellen GPG-Schlüssel von Docker herunter.

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc
```

* Füge das Docker-Repository zu deinen Paketquellen hinzu.

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
```

{% endtab %}
{% endtabs %}

* Aktualisiere die Paketlisten erneut.
```bash
apt update
```

* Installiere Docker Engine, die CLI, containerd, Buildx und das Compose-Plugin.
```bash
apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

* Aktiviere den Docker-Dienst, sodass er auch nach einem Neustart automatisch startet.
```bash
systemctl enable --now docker
```

* Füge deinen Benutzernamen zur Docker-Gruppe hinzu (damit du Docker ohne `sudo` verwenden kannst). Danach einmal ab- und wieder anmelden.
```bash
usermod -aG docker $USER
```

* Überprüfe die Installation mit einem Testcontainer.
```bash
docker run --rm hello-world
```

# Docker Compose verwenden

{% hint style="info" %}
Docker Compose ist mit dem Paket `docker-compose-plugin` bereits installiert und wird über den Befehl `docker compose` (mit Leerzeichen, **ohne** Bindestrich) aufgerufen. Das alte, eigenständige `docker-compose` (v1) wird seit 2023 nicht mehr unterstützt und sollte nicht mehr verwendet werden.
{% endhint %}

* Überprüfe die installierte Compose-Version.
```bash
docker compose version
```

* Eine `docker-compose.yml` startest du anschließend im jeweiligen Verzeichnis so:
```bash
docker compose up -d
```
