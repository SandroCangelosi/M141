# Backup und Restore
# Theorie
- Cold: Bei dem Export und Import werden die Daten für den Endbenutzer nicht zur Verfügung stehen.  
- Warm: Bei dem Export und Import werden die Daten für den Endbenutzer eingeschränkt (z.Bsp. nur lesend) zur Verfügung stehen.  
- Hot: Bei dem Export und Import werden die Daten für den Endbenutzer voll funktionsfähig zur Verfügung stehen.  
- logisch: Bei dem die Daten des Export und Importes in "menschlesbarer Form" (z.Bsp. SQL-Script) zur Verfügung stehen.  
- physisch: Bei dem die Daten des Export und Importes in binärer Form zur Verfügung stehen (z.Bsp. Filesystem-Snapshot).  
- full vs. incremental: Beim Fullbackup wird immer der gesamte Datenbestand gesichert. Beim inkrementellen Vorgehen werden die Änderungen zwischen den Versionen gesichert. Zu Beginn des inkrementellen Backup muss ein Full Backup erstellt werden.  

# Warmes Backup - logisch + full
Um ein logisches full Backup zu erstellen, kann der MYSQL Befehl ```mysqldump``` verwendet werden.  
Es wird in menschenlesbarer Form exportiert. Dazu wird in der Backupzet die Tabelle fürs schreiben gesperrt.  

Bespiel der Erstellung:  
```sql
mysqldump --single-transaction -u root -p --all-databases > dump.sql
```

Beispiel der Wiederherstellung:  
```sql
mysql -u root -p < dump.sql
```

Normalerweise reicht dieses Vorgehen der Backuperstellung.  

## Nachteile 
Folgende Nachteile:  
- Die Backup- und Wiederherstellung dauert lange  
- Die Backups benötigen viel Speicher

## Vorteile
Folgende Vorteile hat dieses Vorgehen:  
- Die Backups liessen sich einfacher auf anderen Produkten (MSSQL, Oracle, usw) und auf anderen Versionen wiederherstellen.  
- Die Informationen sind les- und interpretierbar. D.h. so können Daten anpassen/verändern/korrigieren/reparieren werden.  

## Wichtig zu beachten
Da der Befehl ```mysqldump``` standartmässig alle Befehle nacheindander exportiert, kann es sein das die Daten sich bereits verändert haben (welche bereits exportiert wurden) ohne das der Befehl fertig ist. So erhält man inkosistente Daten. Dies kann mit der Option ```--single-transaction``` verhindert werden.  

# Cold Backup - physisch + full / inkrementell
Als Cold Option könnte man die Daten einfach wegkopieren.

Beispiel:  
```
rsync -avz /var/lib/mysql/* /ZIELVERZEICHNIS/ 
```

oder auch so:  
```
cp -ar /var/lib/mysql/* /ZIELVERZEICHNIS/ 
```

## Vorteile
Diese Variante hat folgende Vorteile:  
- Die Daten von dem Backup sind klein  
- Es wird schnell gesihert  
- Die Memory-Daten werden auch mitgesichert  


## Nachteile
Diese Variante hat folgende Nachteile:  
- Der Dienst muss in dieser Zeit deaktiviert sein  
- Ein exakter Zeitpunkt/Zustand der Daten lässt sich nicht wiederherstellen  
- Der Restore funktioniert oder eben nicht. Keine Korrekturen möglich.  

# Inkrementelles Vorgehen - Point-in-Time-Restore (PITR)
Damit zu jedem möglichen Zeitpunkt in der Vergangenheit zurückgekehrt werden kann muss ein Inkrementelles Backup erstellt werden.

Backup-Erstellung:
- Regelmässig müssen Full-Backup erstellt werden
- Die Änderungen auf der Datenbank zwischen den verschiedenen Backups sichern

Vorgehen bei der Wiederherstellung:  
- Zeitpunkt für die Wiederherstellung finden
- Letztes Full-Backup vor des Wiederherstellungszeitpunkt einspielen
- Alle Änderungen / Inkremmentelen Backups einspielen

## Wichig zu beachten
Für dieses Backup müssen die Binäre Logdaten akiviert werden.

# Migration von Daten
Es wird zwischen physisch und logisch unterschieden, die Vor- und Nachteile bleiben die gleichen.   

Physisch:  
- Vorteil: Schnell und platzsparend  
- Nachteil: Keine Fehlertolerant  

Logisch:  
- Vorteil: Struktur vorhanden, lesbar und veränderbar. Daten können auf andere Produkte migriert werden  
- Nachteil: Gross und langsam  

## Beispiel 1
Die Daten können so migriert werden.  
So kann Zeit gespart werden, dann sonst werden alle diese Schritte einzel und nacheinder ausgeführt.  
Dabei wird von dem Localhost zu dem Remotehost gesendet:  
```sql
    mysqldump --single-transaction -u user -pPASS dbname -h localhost \
    | mysql -u root -pPASS -h 192.168.1.1 DATENBANKNAME
```
*Wichtig: Die Datenübertragung ist unverschlüsselt*  

## Beispiel 2 (SSH + PIPE)
Hier ein Beispiel mittels SSH + PIPE:  
```sql
    mysqldump --single-transaction -u root -pPASS DBNAME | 
    ssh user@SERVER2 'mysql -u USER -pPASS NEUEDATENBANK'
```
*Auf dem Zielserver muss eine DB vorhanden sein*

Noch effizienter geht es mit zippen:  
```sql
ssh user@SERVER1 'mysqldump --single-transaction -u root -pPASS DBNAME | gzip -9' 
    | ssh user@SERVER2 'gunzip | mysql -u USER -pPASS NEWDB'
```

## Info
Backups können zum Beispiel mittel ```crontab``` automatisiert werden.  
So das z. B. jede Woche am Montag ein Full-Backup von einer Applikation gemacht wird.  