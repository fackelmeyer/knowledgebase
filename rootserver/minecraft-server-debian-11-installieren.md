# Minecraft Server auf Debian
In dieser Anleitung könnt ihr lesen, wie ihr einen Minecraft-Server auf eurem Debian-Rootserver (Debian 11, 12 oder 13) installiert.
## Java installieren
Damit euer Minecraft-Server funktionieren kann, benötigt er eine Version von Java. Das JDK (*Java Development Kit*) und die JRE (*Java Runtime Environment*) sorgen dafür, dass euer Server ganz einfach mithilfe des `java`-Befehls gestartet werden kann.

Die benötigte Java-Version richtet sich nach der Minecraft-Version:

| Minecraft-Version | Java-Version |
| --- | --- |
| 1.17 – 1.20.4 | [Java 17](java-17-setup.md) |
| 1.20.5 und neuer (u.a. 1.21.x) | [Java 21](java-21-setup.md) |

1. Die Pakete und Paketquellen müssen aktualisiert werden
```bash
sudo apt-get update && sudo apt-get upgrade -y
```
2. Installiert die zu eurer Minecraft-Version passende Java-Version. Für aktuelle Versionen (1.20.5+ / 1.21.x) ist das **Java 21**.
```bash
sudo apt-get install openjdk-21-jdk openjdk-21-jre -y
```
{% hint style="info" %}
`openjdk-21` ist in Debian 13 enthalten. Unter Debian 12 installiert ihr Java 21 über das Eclipse-Temurin-Repository – die Schritte dazu findet ihr in der Anleitung [Java 21 installieren](java-21-setup.md). Für Minecraft bis 1.20.4 genügt `openjdk-17-jdk openjdk-17-jre`.
{% endhint %}

## Minecraft-Server herunterladen
1. Nun sollte ein Verzeichnis erstellt werden, in welchem später der Minecraft-Server liegt. Daraufhin wird direkt in dieses Verzeichnis gewechselt. Zum Beispiel:
```bash
mkdir /home/minecraft/ && cd /home/minecraft/
```

2. Jetzt sollte die Datei des gewünschten Minecraft-Servers heruntergeladen werden. Dabei kann es sich um Vanilla, Spigot, Paper, Forge, Fabric, usw. handeln. Die neuste Vanilla-Version kann auf der Seite https://www.minecraft.net/de-de/download/server heruntergeladen werden. Um den Prozess zu vereinfachen, sollte die Datei direkt auf den Server heruntergeladen werden. Dazu kann einfach im Rechtsklickmenü des Downloadlinks *"Adresse des Links kopieren"* ausgewählt werden.
![Adresse des Links kopieren](../.gitbook/assets/minecraft-server-download-adresse-kopieren.png)

Dann kann die Datei mit dem `wget`-Befehl heruntergeladen werden. Der kopierte Link lässt sich im Terminal mit Rechtsklick einfügen. Der Befehl sieht dann beispielhaft so aus (ersetzt die URL durch den von euch kopierten Download-Link):
```bash
wget <DOWNLOAD-LINK> -O server.jar
```
{% hint style="info" %}
Für die meisten Server empfiehlt sich **PaperMC** statt Vanilla, da es deutlich performanter ist. Die passende `paper.jar` findet ihr unter https://papermc.io/downloads.
{% endhint %}
3. Dieser Schritt ist nur notwendig, wenn die heruntergeladene Datei nicht `server.jar` heißt. Sie muss dann mit folgendem Befehl umbenannt werden.
```bash
mv <dateiname.jar> server.jar
```
*<dateiname.jar>* muss natürlich mit dem entsprechenden Dateinamen ersetzt werden.

## Server starten
1. Der Start des Servers wird mit dem Anlegen eines Startscripts vereinfacht. Damit der Server im Hintergrund laufen kann, sodass nicht immer ein Terminal geöffnet sein muss, wird zusätzlich die screen-software benötigt.
```bash
sudo apt-get install screen -y
```
2. Das Startscript wird mit diesem Befehl angelegt:
```bash
touch start.sh
```
3. Mithilfe des Texteditors nano wird nun der Startbefehl des Servers in das Startscript eingefügt.
```bash
nano start.sh
```
Ich verwende dafür die folgende Zeile. Sie kann wieder mit Rechtsklick im Editor nano eingefügt werden.
```bash
screen -S minecraft java -Xmx4G -Xms4G -jar server.jar
```
Gespeichert wird die Datei mit der Tastenkombination `STRG + O`. Der Editor nano kann daraufhin mit der Tastenkombination `STRG + X` beendet werden.

4. Das Startscript muss nun die Berechtigung zum Ausführen erhalten. Das funktioniert mit folgendem Befehl:
```bash
chmod +x start.sh
```
5. Zum Start des Servers ist es notwendig, die EULA von Minecraft zu akzeptieren. Hierfür muss die Datei `eula.txt` angelegt werden. Dort wird nur noch die Zeile `eula=true` eingefügt.

6. Der Server kann nun mit der Ausführung des Startscripts gestartet werden.
```bash
./start.sh
```

## Abschluss
Die Serverkonsole kann mit der Tastenkombination `STRG + A` und einem darauffolgenden Tastendruck der Taste `D` verlassen werden. Sie kann mit dem folgenden Befehl wieder aufgerufen werden:
```bash
screen -r minecraft
```
In [diesem](minecraft-auto-start.md) Artikel wird euch gezeigt, wie ihr den Minecraft-Server nach einem Absturz direkt wieder automatisch starten könnt!
