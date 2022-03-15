# Import und Export  
Info:  
MySQL TCP-Port: 3306  

Bei diesem Fehler:
```
mysqldump: Got error: 1290: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement when executing 'SELECT INTO OUTFILE
```

Möglichkeit 1 : Server mit der  "secure-file-priv"-Option starten  
Möglichkeit 2 : Alles unter ```/var/lib/mysql-files/``` speichern    

## Import  
Um SQL Befehle aus einem Skript einzulesen kann dieser Befehl verwendet werden:  
```sql
mysql -u USERNAME -pPASSWORD DBNAME -h HOSTIP < script.sql
```
*-h HOSTIP ist für eine externe DB*

### Import über SQL
```sql
LOAD DATA INFILE '/ordner/datei.csv' 
INTO TABLE country  
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
```

### Import über mysqlimport
```sql
mysqlimport --ignore-lines=1 \ # Header überspringen
            --fields-terminated-by=, \ # Feld-Begrenzungen: Komma oder Semikolons
            --local -u root -p Database \ # Zugriff auf die DB
             TableName.csv # Filename
```

## Export
### Export in ein Skript über mysqldump  
Um aus einer Datenbank die vorhandenen Daten zu exportieren, werdent man diesen Befehl:
```sql
mysqldump [options] --databases db_name ... > dump.sql
```

### Export in eine CSV Datei  
Der Export beginnt mit einem normalen select Befehl.  
Darauf folgt der Into Befehl, mit dem man die CSV Datei definiert.  
```sql
SELECT * From Kunde

INTO OUTFILE '/ordner/ausgabe.csv'
FIELDS ENCLOSED BY '"'  -- Jedes Feld wird mit "" eingefasst
TERMINATED BY ';' -- Feld wird mit einem Semikolon beendet
LINES TERMINATED BY '\n';
```
*"\n" = Linux und "\r\n" für Windows*

### Export in eine CSV Datei mittels mysqldump
```sql
mysqldump DATENBANK TABELLE --fields-terminated-by ',' \
--fields-enclosed-by '"' --fields-escaped-by '\' \
--no-create-info --tab /var/lib/mysql-files/
```