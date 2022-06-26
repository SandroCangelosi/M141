# Datenbankdesign
Beim Datenbankdesign müssen folgenede Fragen beachtet werden:  

Wird in der Applikation viel gelesen / geschrieben?    
Welchen Datensätze werden oft aufgerufen?  
Wie hoch soll die Performance sein?  
Wie werden sich die Daten verändern und anwachsen?

## Relational vs. Schemaless
**Relational:** Daten werden in verschiedene Tabellen aufgeteilt.  
Joins werden verwendet um Daten aus Tabellen hinzuzufügen.  

**Schemaless:** Daten werden in Array und Nested Documents gespeichert.  
Hier werden keine separaten Collections erstellt.  

## Embedding vs. Referencing
**Referencing:** "Foreign-Key - Public-Key"-Vorgehen. Es wird der Operator $lookup dafür verwendet.  
Vorteile:  
- Kleinere Dokumente  
- Dokumente können kleiner als das Limit 16MB sein  
- Ist gut, Wenn die referenzierten Daten selten gebraucht werden  
- Weniger Redundanz  

Nachteile:
- Abfragen werden grösser und komplizierter

**Embedding:** In den Anwendungen werden zusammengehörige Informationen im selben Datenbankeintrag gespeichert.  
Vorteile:  
- Alle Daten werden in einer Query geladen  
- Es werden keine Joins verwendet  
- Alle CRUD-Operationen von einem Document sind ACID-kompatibel  

Nachteile:  
- Eine grössere Query auf viele Dokumente kann durch die doppelten Daten die Performance beeinflussen.  

## Wichtigste Regeln
1. Generell sollen Embedded Dokumente verwendet werden, ausser es gibt gute Gründe dagegen  
2. Wenn man ein einzelnes Document einzeln anzeigen zu möchte, sollte man es nicht zu embedden  
3. Generell Joins/Lookups nach möglichkeit vermeiden. Hilft es allerdings ein performanteres Schema zu erreichen, dann soll man es trotzdem verwenden    
4. Nested Arrays sollten nicht "unbegrenzt" gross werden können  
5. Many-To-Many - hier muss mit Referenzen gearbeiten werden  
