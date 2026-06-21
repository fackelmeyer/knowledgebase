# Installation von MongoDB auf einem Linux-Server

MongoDB ist eine kostenlose, quelloffene, dokumentenorientierte Datenbank, die Daten in JSON-ähnlichen Dokumenten mit einem flexiblen Schema speichert. Diese "NoSQL"-Datenbank ist eine beliebte Alternative zu klassischen relationalen Datenbanken wie MySQL.

In dieser Anleitung installierst du die **MongoDB Community Edition 8.0** (aktuelle stabile Major-Version) über das offizielle MongoDB-Repository.

{% hint style="warning" %}
MongoDB wird offiziell für **Debian 12 (bookworm)** sowie **Ubuntu 22.04 (jammy)** und **Ubuntu 24.04 (noble)** unterstützt. Für Debian 13 (trixie) stellt MongoDB derzeit noch kein vollständiges Server-Paket bereit.
{% endhint %}

{% hint style="warning" %}
MongoDB 5.0 und neuer (also auch 8.0) benötigt eine CPU mit **AVX-Unterstützung** (Intel Sandy Bridge / AMD Bulldozer oder neuer). Auf älteren oder stark eingeschränkten virtuellen Servern ohne AVX startet `mongod` nicht.
{% endhint %}

* Aktualisiere zuerst die Paketlisten und installiere die benötigten Pakete.
```bash
apt update && apt install gnupg curl -y
```

* Importiere den offiziellen GPG-Schlüssel von MongoDB.
```bash
curl -fsSL https://pgp.mongodb.com/server-8.0.asc | gpg --dearmor -o /usr/share/keyrings/mongodb-server-8.0.gpg
```

{% tabs %}
{% tab title="Debian 12" %}
* Füge das MongoDB-Repository hinzu.

```bash
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/debian bookworm/mongodb-org/8.0 main" | tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```
{% endtab %}

{% tab title="Ubuntu 22.04" %}
* Füge das MongoDB-Repository hinzu.

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```
{% endtab %}

{% tab title="Ubuntu 24.04" %}
* Füge das MongoDB-Repository hinzu.

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```
{% endtab %}
{% endtabs %}

* Aktualisiere die Paketlisten erneut.
```bash
apt update
```

* Installiere MongoDB. Das Meta-Paket `mongodb-org` zieht alle benötigten Komponenten (Server, Shell, Tools) mit.
```bash
apt install mongodb-org -y
```

* Starte den MongoDB-Dienst und aktiviere den automatischen Start nach einem Neustart.
```bash
systemctl enable --now mongod
```

* Überprüfe, ob der Dienst läuft.
```bash
systemctl status mongod
```

* Du kannst dich anschließend mit der MongoDB-Shell verbinden.
```bash
mongosh
```

## Zugriff absichern

Standardmäßig ist die Authentifizierung deaktiviert und MongoDB lauscht nur auf `localhost` (`127.0.0.1`). 

{% hint style="danger" %}
Öffne MongoDB **niemals** ungesichert nach außen. Lege zuerst einen Administrator-Benutzer an und aktiviere die Authentifizierung, bevor du `bindIp` in der Datei `/etc/mongod.conf` änderst. Beschränke den Zugriff zusätzlich über eine Firewall (z.B. UFW) auf vertrauenswürdige IP-Adressen.
{% endhint %}

* Verbinde dich mit der Shell und lege einen Admin-Benutzer an.
```bash
mongosh
```
```javascript
use admin
db.createUser({
  user: "admin",
  pwd: passwordPrompt(),
  roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
})
exit
```

* Aktiviere danach die Authentifizierung in der Datei `/etc/mongod.conf`, indem du den folgenden Abschnitt ergänzt bzw. einkommentierst.
```yaml
security:
  authorization: enabled
```

* Starte den Dienst neu, damit die Änderungen wirksam werden.
```bash
systemctl restart mongod
```
