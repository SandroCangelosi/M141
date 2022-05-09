# DB-Benutzern Authentifizierung, Autorisierung und URIs
## Github Repository in Betrieb nehmen

```bash
git clone https://github.com/sbhaseen/express-dashboard-demo.git
```

```bash
cd express-dashboard-demo
```

Nach Anleitung hätte dieser Befehl NPM installieren müssen, das hat aber bei mir nicht geklappt.  
```bash
npm install
```
*Error: Command not found*

Dann habe ich im Internet folgende Lösung gefunden:  
```bash
sudo apt-get install npm
```

Um die Applikation schlussendlic zu Starten, muss dieser Befehl verwendet werden:  
```
npm run dev
```

Dann kann man mit `http://localhost:5000` im Browser auf die Applikation zugreifen.  
![Installationstest](installationstest.png)  


## Testen  
Erfassen Sie auf der Applikation einzelne Datensätze (z.Bsp. eine neue Maschine) und verifizieren Sie folgende Dinge:  

### Über welchen Controller wird kommunziert?  
Für jede Erstellung von einem Objekt wird der entsprechende Controller verwendet.
![Controllers](controllers.png)  

In den Controllen drinnen, kann dann ein Objekt erstellt werden.  
Bsp. hier von dem Controller Category:  
![Category](category.png)  

Oder hier von dem Controller Processinstance:  
![Processinstance](processinstance.png)  

### Über welches Model wird der Datensatz in der Datenbank gespeichert?  
Für jedes Objekt gibt es wieder eine eigene Klasse / Datei mit den eigenen Methoden.   
![Models](models.png)  

In der letzten Zeile wird der Datensatz in die DB exportiert:  
![Daten Export](export.png)  

### Wie sehen die Dokumente in der Datenbank aus?  
In der `mfgdashboard` DB werden die Dokumente gespeichert. Die Dokumente werden in den `Collections` verwaltet.

Beispiel:  
![Beispiel der Dokumente](documents.png)  

##  Anwendung modifizieren 
### Erfassen Sie auf der MongoDB-Instanz einen Systemadministrator
Damit man sich jetzt autorisieren muss, muss zu erst die `authorization` in der `mongod.conf` unter `/etc/mongod.conf`:  
```
security:
    authorization: enabled
```

Hier wird der Administrator `myUserAdmin` mit dem Passwort aus der Eingabeaufforderung in der MongoDB Shell erstellt:  
```sql
use admin
db.createUser(
{
    user: "myUserAdmin",
    pwd: passwordPrompt(), // oder cleartext password. passwordPrompt() führt zu einem Prompt auf der Konsole
    roles: [
    { role: "userAdminAnyDatabase", db: "admin" },
    { role: "readWriteAnyDatabase", db: "admin" }
    ]
}
)
```

Mit diesem Befehl kann man die MongoDB instaz stoppen:  
```sql
db.adminCommand( { shutdown: 1 } )
```

Dann kan man sich mit dem Admin anmelden:  
```sql
mongo --port 27017  --authenticationDatabase \
"admin" -u "myUserAdmin" -p
```
*Mit -p wird man in der Shell nach dem Passwort gefragt*
### Erfassen Sie auf der MongoDB-Instanz einen Applikationsbenutzer für diese Applikation  
Wenn man eine benutzerspezifische Rolle definieren:  
```sql
use admin
db.createRole(
   {
     role: "manageOpRole",  // Name der neuen Rolle
     // privileges erfassen -> jeweils eine Kombination aus action und ressource
     privileges: [
       { resource: { cluster: true }, actions: [ "killop", "inprog" ] },
       { resource: { db: "", collection: "" }, actions: [ "killCursors" ] }
     ],
     roles: []
   }
)
```

Dann konnte ich den Benutzer mit der Rolle hinzufügen möchte, muss dies davor geschehen:  
```sql
use admin
db.createUser(
{
    user: "mfgdbuser",
    pwd: "mfgdbuser", 
    roles: [
    { role: "manageOpRole", db: "admin" },
    { role: "readWriteAnyDatabase", db: "admin" }
    ]
}
)
```
### Passen Sie die URI in der Datei server.js entsprechend an

In der `server.js` Datei habe ich den URI Befehl auf folgendes geändert:  
```
mongodb://myUserAdmin:myUserAdmin@localhost:27017/mfgdashboard?authSource=admin
```  

![Server.js Beispiel](serverjs.png)  

Dann konnte ich mich auch wieder verbinden (Mit Localhost und den Benutzern):  
![Verbindung](verbindung.png)  

