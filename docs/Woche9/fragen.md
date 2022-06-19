# Fragenpool für die Prüfung
## LB 1 (schriftliche Arbeit, schriftlicher Test)
### Tag 2
Recherche Auftrag und Einschätzung von einem "unbekannten" DBMS  
- Hier sind ein paar Beispiele von unbekannten DBMS und eine Kategorisierung von ihnen:  
    [Geschichte](/Woche1/geschichtedb?id=heutige-systeme-dbms)  

### Tag 3
Durchführung einer Installation  
- Hier eine Hilfe für die MySQL installation:  
    [MySQL Installation](/Woche3/mysql?id=prozess-der-installation-wiewas-habe-ich-es-installiert)  

### Tag 4
Anwendung von Storage Engines auf eine Tabelle  
- Hier werden Storage Engines beschrieben:  
    [MySQL Installation](/Woche4/mysqlkonfig?id=storage-engines-bei-mysql) 

Anwendung von Security/Berechtigungen auf eine Umgebung  
- Hier wird die Erstellung von Berechtigung beschrieben:  
    [Berechtigung](Woche4/mysqlkonfig?id=benutzer-und-berechtigungen) 

Transkationsisolation anwenden und konfigurieren  
- Hier habe ich die Transkationsisolation beschrieben:  
    [Transkation Isolation](Woche4/mysqlkonfig?id=benutzer-und-berechtigungen) 
- Hier nochmals
    [Transkation](/Woche2/transaktion.md)

Protokollierung langsamer Abfragen aktivieren  
- Da ich keine Lösung im Skript dokumentiert habe, habe ich es jetzt nachträglich hinzugefügt:  
    Checken, ob die langsame Protokollierung aktiviert ist:  
    ```sql
    SELECT @@slow_query_log;
    ```

    Zeit abfragen:  
    ```sql
    SELECT @@long_query_time;
    ```

    Zeit einstellen (0-10):
    ```sql
    SET GLOBAL slow_query_log=0;
    ```


    Die Konfig Datei ist gespeichert unter:
    /etc/mysql/mysql.conf.d/mysqld.cnf

    slow_query_log = 1;

### Tag 5
Umsetzung Referenzielle Integrität  
- Hier habe ich alles über Primär und Fremdschlüssel beschrieben:  
    [Primär und Fremdschlüssel](/Woche3/erm_erd.md)  
    Es kann ken Datensatz gelöscht werden, welcher als Primarschlüssel verwendet wird und jemand auf diesen Referenziert.  
    Dazu kann es nicht auf etwas referenziert werden, was es nicht gibt.  


Indexierungstypen aufgrund Anforderungen umsetzen  
- Hier habe ich alles über das Indexes beschrieben:  
    [Indexing](Woche5/index.md) 

Datentypen sinngemäss richtig anwenden  
- Hier habe ich alles über die Datentypen beschrieben:  
    [Datentypen](Woche5/datentypen.md) 

### Tag 6
Auftrag : Import und Export von verschiedenen Datentypen  
- Hier steht alles zum Thema Import und Export:  
    [Import und Export](Woche6/import_export.md) 

### Tag 7
Auftrag : Durchführung von Backup/Restore (nicht PITR)  
- Hier steht alles zum Thema Backup und Restore:  
    [Backup und Restore](Woche6/backup_restore.md) 

Auftrag : Migration von Daten durchführen  
- Alles über Migration steht hier:  
    [Backup und Restore](Woche6/backup_restore?id=migration-von-daten) 

### Tag 8
Auftrag : Beispielapplikation installieren und Datenauslesen  
- Hier ist alles zum Pratischen Beispiel von Etherpad dokumentiert:  
    [Etherpad](Woche7/etherpad) 

### Tag 9
Auftrag : Stored Procedures importieren, anzeigen, überarbeiten  
Auftrag : Stored Procedures anwenden  
- Hier ist alles zum Thema Stored Procedures dokumentiert:  
    [Stored Procedures](Woche8/theorie?id=stored-procedures)  
    [Praktisches Beispiel Stored Procedures](Woche8/sakila?id=anzeigen)  

Auftrag : Views importieren, anzeigen, überarbeiten  
Auftrag : Views anwenden  
- Hier ist alles zum Thema Views dokumentiert:  
    [Views](Woche8/theorie?id=views)  
    [Praktisches Beispiel Views](Woche8/sakila?id=anzeigen)  

Auftrag : Triggers importieren, anzeigen  
- Hier ist alles pratische zum Thema Triggers dokumentiert:  
    [Triggers](Woche8/sakila?id=anzeigen)  

### Tag 10
Auftrag : (Theorie) Schlechte Query - verbessern Sie die Performance  
Auftrag : (Praxis) Schlechte Query - verbessern Sie die Performance  
- Hier ist alles über schlechte Querys dokumentiert:  
    [Performance](Woche9/performance.md)  

## LB 2 (Mündliche Prüfung)
### Tag 2
Erklären Sie den Unterschied zwischen ACID und BASE  
* [ACID](Woche2/transaktion?id=acid)  
* [BASE](Woche2/transaktion?id=base)  

Erklären Sie das CAP-Theorem mit hilfe von Beispiel-DBMS  
* [CAP-Theorem](Woche2/transaktion?id=cap-theorem)  

Erklären Sie was Transaktionen sind und was Anomalien sind. Erklären Sie in diesem Zusammenhang die Einstellungen zur Performancesteigerung  
* [Anomalien](Woche2/transaktion?id=probleme-anomalien-wenn-man-nicht-mit-acid-arbeitet)  
* [Transaktionen](Woche2/transaktion?id=transaktionen-dokumentationsauftrag)  

### Tag 3
Erklären Sie mysql_secure_installation  
Erklären Sie wie Linux in Kombination mit MySQL-Versionen funktioniert  
* [mysql_secure_installation](Woche3/mysql?id=prozess-der-installation-wiewas-habe-ich-es-installiert)  

### Tag 4
Erklären Sie was Storage Engines in MySQL sind. Benennen und beschreiben Sie die zwei wichtigsten Exemplare  
Wo die Daten bei MySQL effektiv gespreichert werden  
* [MySQL Installation](/Woche4/mysqlkonfig?id=storage-engines-bei-mysql)  
* Daten von MySQL werden unter "/var/lib/mysql" gepeichert  

### Tag 5
Erklären Sie FK und PK Indexierungen  
Erklären Sie die die Datentypen JSON, ENUM, XY und machen Sie ein Beispiel für die Anwendung dieser Datenypen  
* [Indexing](Woche5/index.md)  
* [Datentypen](Woche5/datentypen.md)  

### Tag 6
Vorgehen erklären beim Import und Export  
    Verschiedene Aufgabenstellungen vorformuliert  
* [Import und Export](Woche6/import_export.md) 


### Tag 7
Verschiedene Backupvarianten vergleichen und argumentieren  
Verschiedene Migrationsvorgehen erläutern und vergleichen können  
* [Backup und Restore](Woche6/backup_restore.md) 

### Tag 8
Angriffsvektoren anhand von einer Beispielapplikation analysieren und erläutern  
* [Berechtigungen](Woche7/berechtigung.md) 

### Tag 9
Beispiel-Importdatei auf Stored Procedures, Views und Triggers interpretieren  
Stored Procedures: Sinn, Anwendung und Syntax erläutern  
Views: Sinn, Anwendung und Syntax erläutern  
Triggers: Sinn, Anwendung und Syntax erläutern  
* [Stored Procedures](Woche8/theorie?id=stored-procedures)  
* [Praktisches Beispiel Stored Procedures](Woche8/sakila?id=anzeigen)  
* [Views](Woche8/theorie?id=views)  
* [Praktisches Beispiel Views](Woche8/sakila?id=anzeigen)  
* [Triggers](Woche8/sakila?id=anzeigen)  

### Tag 10
Auftrag : (Theorie) Schlechte Query - verbessern Sie die Performance  
* [Performance](Woche9/performance.md)  