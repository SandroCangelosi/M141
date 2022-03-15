# SQL
## Wiederholende Scripts
Falls bei einem wiederholendem Script sich der Befehl nicht ausgeführt werden soll.  
Dann kann man das mit `IF NOT EXISTS` machen.  

Beispiel:
```sql
CREATE DATABASE IF NOT EXISTS
CREATE TABLE IF NOT EXISTS
```

## Primary und Foreign Keys
### Primärschlüssel
Bei einem enzelnen Attribut:  
Die Storage Engine wird nach der Erstellung festgelegt.   
```sql
CREATE TABLE IF NOT EXISTS Kunde (
    KundeID INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    PRIMARY KEY (KundeID)
) ENGINE=InnoDB;
```

Bei mehreren Attributen:  
```sql
CREATE TABLE IF NOT EXISTS Kunde (
    KundeID INT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    PRIMARY KEY (KundeID, name)
) ENGINE=InnoDB;
```

Im nachhinein den PK bearbeiten (löschen und dann neu erstellen):  
```sql
ALTER TABLE Kunde DROP PRIMARY KEY; 
ALTER TABLE Kunde ADD PRIMARY KEY (KundeID, name);
```

### Fremdschlüssel
Fremdschlüssel bei diesem Beispiel definieren:  
```sql
CREATE DATABASE IF NOT EXISTS dbdemo;

USE dbdemo;

CREATE TABLE categories(
   cat_id int not null auto_increment primary key, -- PK definieren 
   cat_name varchar(255) not null
) 

CREATE TABLE products(
   prd_id int not null auto_increment primary key,
   prd_name varchar(355) not null,
   cat_id int not null, -- Atribut für den FK definieren 
   FOREIGN KEY fk_cat(cat_id) REFERENCES categories(cat_id) -- Referenz setzen
        ON UPDATE CASCADE -- was passieren soll wenn die Referenz geupdated wird
        ON DELETE RESTRICT -- was passieren soll wenn die Referenz gelöscht wird
)
```

1. RESTRICT oder NO ACTION:  
- Update oder Delete wird von der Referenztabelle(bei der der PK ist) verboten.  
2. CASCADE:  
- Wird auf der Referenztabelle ein Datensatz gelöscht, werden auch alle Childs gelöscht.  
3. SET NULL:  
- Wird auf der Referenztabelle ein Datensatz gelöscht, werden die FKs einfach auf NULL gesetzt.  