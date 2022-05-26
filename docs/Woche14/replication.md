# Replication bei MySQL
## Info - deterministisch vs. nicht deterministisch
`Deterministisch` ist eine Zahl, welche sich nicht verändert.  
So ist dann `nicht deterministisch` eine Zahl die sich verändern kann.  

Beispiel:  
**Nicht deterministische** -> Alter (verändert sich jedes Jahr)  
**Deterministische** -> Geburtstagdatum  

## Replikation Grundsätzlich
Zwischen dem Source Server und den Rplica Nodes werden die Änderungen über Binary Logs (Binäre Log Dateien) übertragen.  

* Statement-based  
    - Alle Befehle werden auf dem Replica wiederholt. Bei grossen `nicht deterministischen` Queries kann es zu unterscheidlichen Datensätzen kommen.   

* Row-based
    - Hier wird übergeben, was geändert wurde. So kann es sein, dass die Binary Log sehr gross werden.  
  
* mixed
    - Hier werden beide Vorteile von oben genommen und verwendet. So wird generell `statement-based` gearbeitet und bei nicht-deterministischen Aufgaben wird `row-based` verwendet.  
  
### Aktiv / Passiv
Bei dem Aktiv / Passiv Beispiel werden Daten auf den Source Server geschrieben. Dazu werden Daten von diesem Server aus gesendet.  
Die Replikationen Nodes können nur für eine Replikation von den Daten gebraucht werden. Die Daten können als Backup verwendet werden.  
![Aktiv und Passiv Beispiel](passiv.png)  

### Aktiv / Read Pool
Bei dem Aktiv / Lese Pool ist es so, dass die Daten auf den Source Server gespeichert werden und von den Replikations Nodes werden die Daten gelesen.  
Die Daten werden von dem Source Server zu den Replikations Nodes repliziert.  
Dies Erhöht die Performance besonders für die Read Abfragen. Das Schreiben kann natürlich auch positiv beeinflusst werden.   
![Aktiv und Read Pool Beipsiel](read.png)  

### Wählen des Clusters
* Hohe Lese Lasten (Select Abfragen) -> Replikation mit Read-Pools  
* Hohe Schreib Lasten (Insert / Update / Delete) -> Sharding 

## Aufträge    
### Auftrag 1 - Analyse docker-compose.yml  
Welche Services werden aufgebaut? -> Zeichnen Sie ein kleines Netzwerkdiagramm (irgendwie)  
![Netzwerkplan Services](netzwerkplan_klein.png)  

Welche Netzwerk werden aufgebaut? -> Erweitern Sie das Diagramm  
![Netzwerkplan Services und Aufbau](netzwerkplan.png)  

In der Datei config/master.cnf sehen Sie die Konfiguration für den Master-Node. Dokumentieren!  
![Master Config](master.png)  

In der Datei config/replica.cns sehen Sie die Konfiguration für den Slave-Node. Dokumentieren!  
![Replica Config](replica.png)  

## Konfiguration Replikation
### Master
So könnte das Config File aussehen:  
![Config Master](config1.png)  
### Slave
So könnte das Config File aussehen:  
![Config Slave](config2.png)  

## Proxies
Hier die Funktionen und Anforderungen an einen Proxy.  

Funktionen:  
* Caching  
* Blocking  
* Monitoring  
* Rate Limiting  

Anforderungen:  
* hochverfügbar
* skalierbar
* leichtgewichtig sein
* automatisiert konfigurierbar sein