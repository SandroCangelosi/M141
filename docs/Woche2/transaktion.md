# Transaktionen + Dokumentationsauftrag
Eine Transaktion ist eine Folge von SQL-Anweisungen / Aktionen / Operationen, die als Einheit ausgeführt (Commit) oder zurückgenommen (Rollback) werden.  
So das eine Transaktion entweder ganz oder garnicht ausgeführt wird. Damit bei einem Ausfall, die Aktionen welche bereits in der Transaktion ausgeführt wurden zurückgängig gemacht werden können -> Rollback (Falls die Transaktion noch nicht feritg ausgeführt wurde).

[Transaktions und ACID Video](https://www.youtube.com/watch?v=ZHq5f7eZPwA)  
# ACID
Relationale DBMS sind normalerweise ACID Systeme. Trotzdem ist es schwierig komplett ACID zu sein.  
Es beschreibt die Eigenschaften welche man an Transaktionen in Datenbankmanagementsystemen und verteilten Systemen stellt.  


Buchstabe | Bedeutung | Beschreibung
-------- | -------- | --------
A | Automarität | Alle Aktionen aus dieser Transaktion werden ausgeführt oder keine (Alles oder nichts).
C | Konsistenz | Eine / mehrere Regeln, damit die DB nach der Transaktion in einem konsistenten Zustand ist.
I | Isolation | Die einzelnen Transaktionen behindern / beinflussen sich gegenseitig nicht. Die Lösung ist serialisierung -> Eintelne Teile einer Transaktion nacheinander und nicht parallel ausführen.
D | Dauerhaftigkeit | Die erfolreichen Transaktionen müssen dauerhaft in der DB gespeichert werden. Damit die Daten vor einem Ausfall sicherlich auf der Festplatte gespeichert wurden und nicht beim Ausfall im RAM gelöscht werden. 

# CAP-Theorem
Das CAP-Theorem besagt, das es in einem verteiltem System unmöglich ist jede von diesen 3 Eigenschaften gleichzeitig zu erfüllen.  
Man bringt es hin 2 Eigenschaften im System zu erfüllen, jedoch muss man bei der letzten Eigenschaft abstriche machen.   
Falls man als Beispiel die Konsistenz und die Verfügbarkeit benötigt, muss man Abstriche bei der Ausfalltoleranz machen.  

Buchstabe | Bedeutung | Beschreibung
-------- | -------- | --------
C | Konsistenz | Alle Systeme müssen dieselben Daten repliziert haben. Damit man, egal wo man ist und auf welches System man zugreift steht aktuelle Daten hat.
A | Verfügbarkeit | Das System muss stehts Verfügbar sein, damit alle Anfragen bearbeitet werden können.
P | Ausfalltoleranz | Das ganze System funktioniert weiter, auch wenn ein einzelner Netzknoten ausfällt.


# BASE
Das theoretische Gegenstück von ACID, wobei BASE für "`B`asically `A`vailable `S`oft state `E`ventual Consistency" steht.  
Das bedeutet, das die Daten grundsätzlich verfügbar sind. Aber evt. verloren gehen können. Möglicherweise sind die Daten auch konsistent gespeichert.  
Datenbank-Hersteller arbeiten eher nach BASE, da man dadurch Daten gut, schnell und konsistent zu speichern kann.

## Probleme (Anomalien) wenn man nicht mit ACID arbeitet
Wenn man nicht mit ACID arbeitet, können folgende Probleme auftauchen.  
Man kann diese Probleme lösen, jedoch wird dann auch die Performance schwächer.  

Problem | Beschreibung
-------- | --------
Dirty Read | Daten einer noch nicht abgeschlossenen Transaktion werden von einer anderen Transaktion gelesen.
Lost Updates | Zwei Transaktionen modifizieren parallel denselben Datensatz und nach Ablauf dieser beiden Transaktionen wird nur die Änderung von einer von ihnen übernommen.
Non-Repeatable Read | Wiederholte Lesevorgänge liefern unterschiedliche Ergebnisse.
Phantom Read | Suchkriterien treffen während einer Transaktion auf unterschiedliche Datensätze zu, weil eine (während des Ablaufs dieser Transaktion laufende) andere Transaktion Datensätze hinzugefügt, entfernt oder verändert hat. 

Optimiertung / Lösungen für das Relationale Databank Management System:

Isolationsebene | Dirty Read | Lost Updates | Non-Repeatable Read | Phantom
-------- | -------- | -------- | -------- | -------- 
Read Uncommitted | möglich | möglich | möglich | möglich 
Read Commited | unmöglich | möglich | möglich | möglich 
Repeatable Read | unmöglich | unmöglich | unmöglich | möglich 
Serializable | unmöglich | unmöglich | unmöglich | unmöglich 
