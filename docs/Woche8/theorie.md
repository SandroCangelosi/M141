# Theorie - Stored Procedures, Views und Triggers
## Views

Grundidee:  
1. Sensitive Daten zu schützen bzw. nicht allen Zugänglich zu machen.  
2. Komplizierte Anfragen in eine View zusammenfassen.  

Views sind Sichten welche auf eine Tabelle oder Views Abfragen ausführen. Sie werden also verwedet um Daten anzeigen.

Es können Spalten und Attribute mit einer View freigegeben bzw. gesperrt werden. Aber keine einzelnen Datensätze.

### View erstellen
Einfaches Beispiel:  
```sql
CREATE OR REPLACE VIEW Kundensicht AS SELECT * FROM Webseite:  
```
*WICHTIG: Views dürfen nicht gleich heissen wie die Tabellen,*  
*Die View wird, falls sie bereits vorhanden ist überschrieben.*  

View mit mehreren Spalten erstellen:  
```sql
CREATE VIEW Kundenansicht (pok_id, pok_name) AS SELECT pok_id, pok_name FROM pokemon;  
```

### Views anzeigen
Die Views werden mit den Tabellen aufgelistet:  
```sql
SHOW TABLES;
```
  
Die einzelne View genauer anschauen bzw. beschreiben lassen:   
```sql
SHOW CREATE VIEW Kundenansicht;
```

Ausgabe:  
```sql
 SHOW CREATE VIEW Kundenansicht;
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| View            | Create View                                                                                                                                                                                                        | character_set_client | collation_connection |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| Kundenansicht | CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `Kundenansicht` (`pok_id`,`pok_name`) AS select `pokemon`.`pok_id` AS `pok_id`,`pokemon`.`pok_name` AS `pok_name` from `pokemon` | utf8mb4              | utf8mb4_0900_ai_ci   |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
1 row in set (0.00 sec)
```

### View löschen
Eine View löscht man so:  
```sql
DROP VIEW Kundenansicht;
```

### Daten aus einer View anzeigen lassen
So können Daten aus einer View angezeigt werden:  
```sql
SELECT * FROM Kundenansicht;
```

## Stored Procedures
Stored Procedures werden für die Sicherheit verwendet mit dem Sinn, das Daten geändert werden können.  
Denn der Client kann in der Regel keine DELETE -, UPDATE - oder INSERT Rechte bekommt.  
Dazu werden gezielte Redundanzen für die Geschwindigkeit eingebaut.  

### Stored Procedures erstellen
Beispiel:  
```sql
DELIMITER //
CREATE PROCEDURE zurueckgabetabelle (OUT ret_wert INT)

BEGIN
SELECT COUNT(*) INTO ret_wert from tabelle;
END //

DELIMITER ;
```
*In den Klammern wird definiert ob es eine Eingabe oder Ausgabe Parameter ist, dazu wird der Wert mit dem Datentyp definiert*


### Stored Procedures verwenden
So werden Stored Procedures aufgerufen:  
```sql
CALL zurueckgabetabelle(@a);
```
*Das Resultat wid in der globalen Variable @a gespeichert.*

Globale Varable anzeigen:  
```sql
SELECT @a;
```

Falls bei der Verwendung von Stored Procedures ein Fehler auftreten sollte, sieht man es mit folgendem Befehl:  
```
SHOW WARNINGS;
```

### Stored Procedures auflisten  
Alle Stored Procedures auflisten, welche in diese DB sind:  
```sql
SHOW PROCEDURE STATUS WHERE db = 'datenbankname';
```

### Stored Procedures löschen
```sql
DROP PROCEDURE IF EXISTS zurueckgabetabelle;
```

### Stored Procedures beschreiben  
Die Stored Procedures beschreiben:  
```sql
SHOW CREATE PROCEDURE name_von_procedure;
```

### Berechtigung auf eine Storage Procedure geben
Die Berechtigungen kann man mit dem unterem Befehl geben.  

In Mysql ausführen:  
```sql
GRANT EXECUTE ON PROCEDURE db.storage_procedure TO 'user'@'localhost';
```

## Trigger
Ein Trigger ist ein benutzerdefinierter SQL-Befehl, der während eines INSERT -, DELETE - oder UPDATE -Vorgangs automatisch aufgeführt wird.  
Der Trigger wird ausgeführt, wenn ein bestimmter Code Abschnitt durchlaufen ist.  Die Triggers werden hautpsächlich für Redundazen verwednet.  

## Funktionen 
Um alle Funktionen einer DB anzeigen zu lassen kann dieser Befehl verwendet werden:  
```sql
SHOW FUNCTION STATUS WHERE db = 'datenbank_name';
```