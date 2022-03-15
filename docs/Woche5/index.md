# Indexing
## Was ist ein Index
Ein Index hat das Ziel, eine Suche mittels WHERE oder ORDER BY-Statement zu beschleunigen.  
Denn bei gösseren Tabellen mit 1000 - 10000 oder mehr Datensätzen wird die performance sonst immer schlechter.  
Es wird eine spezielle Datenstruktur von der Spalte erstellt, durch die man dann bessere performance bei einer Abfrage hat.   
## Welche Indexes gibt es und wann werden sie verwendet?
Folgende Möglichkeiten gibt es:  
1. Primary Key:  
- Jeder Eintrag in dieser Spalte muss eindeutig sein, keine doppelten Werte.  
- Maximal einen Primary Key pro Tabelle darf es geben.  
2. Unique:  
- Jeder Eintrag in dieser Spalte muss eindeutig sein.  
- keine doppelten Werte.  
- Darf mehrere Unique Werte geben.  
3. Index:  
- Kann auch doppelte Werte in der Spalte enthalten.  
4. Fulltext:  
- Für längere Texte.  
- Jedes Wort wird indiziert und eine Suche nach einzelnen Wörtern oder Teilwörtern ist möglich.  
- Für eine Volltextsuche in MYSQL.  

**Der Vorteil ist, dass schnelle Auffinden von Datensätzen sofern nach der Spalte gesucht wird.**  
**Der Nachteil ist, dass zusätzliche Speicherplatz benötigt wird und INSERT, UPDATE und DELETE-Statements ggf. länger benötigen.**

## Wo erstellt man Indexes
Grundsätzlich gibt es zwei Möglichkeiten.  

1. Über PHPmyAdmin  
2. In der Konsole  
```sql
CREATE UNIQUE INDEX index_name
ON table_name (spalte1, spalte2);
```