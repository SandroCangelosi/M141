# Sakila
## Übung
1. Das Zip File von (https://downloads.mysql.com/docs/sakila-db.zip)[downloads.mysql.com] herunterladen und entpacken.  
2. Datenbank auswählen:   
```sql
USE sakila;
```
3. Testen
- Alle Tabellen mit dem Typ anzeigen:  
```sql
SHOW FULL TABLES IN datanbank_name WHERE TABLE_TYPE LIKE 'VIEW';
```
Ausgabe:  

    ```sql
    +----------------------------+------------+
    | Tables_in_sakila           | Table_type |
    +----------------------------+------------+
    | actor                      | BASE TABLE |
    | actor_info                 | VIEW       |
    | address                    | BASE TABLE |
    | category                   | BASE TABLE |
    | city                       | BASE TABLE |
    | country                    | BASE TABLE |
    | customer                   | BASE TABLE |
    | customer_list              | VIEW       |
    | film                       | BASE TABLE |
    | film_actor                 | BASE TABLE |
    | film_category              | BASE TABLE |
    | film_list                  | VIEW       |
    | film_text                  | BASE TABLE |
    | inventory                  | BASE TABLE |
    | language                   | BASE TABLE |
    | nicer_but_slower_film_list | VIEW       |
    | payment                    | BASE TABLE |
    | rental                     | BASE TABLE |
    | sales_by_film_category     | VIEW       |
    | sales_by_store             | VIEW       |
    | staff                      | BASE TABLE |
    | staff_list                 | VIEW       |
    | store                      | BASE TABLE |
    +----------------------------+------------+
    23 rows in set (0.00 sec)
    ```
  
*Bei 23 Spalten ist die Installation korrekt*

- Dateneinträge zählen
```sql
SELECT COUNT(*) FROM film;
```  
Ausgabe:  

    ```sql
    +----------+
    | COUNT(*) |
    +----------+
    |     1000 |
    +----------+
    1 row in set (0.00 sec)
    ```
*Bei 1000 Einträgen ist die Installation korrekt*

- Dateneinträge zählen
```sql
SELECT COUNT(*) FROM film_text;
```  
Ausgabe:  

    ```sql
    +----------+
    | COUNT(*) |
    +----------+
    |     1000 |
    +----------+
    1 row in set (0.00 sec)
    ```  
*Bei 1000 Einträgen ist die Installation korrekt*

## Anzeigen
Alle Vies von der DB anzeigen:  
```sql
show full tables where table_type = 'VIEW';
```
Ausgabe:  

```sql
+----------------------------+------------+
| Tables_in_sakila           | Table_type |
+----------------------------+------------+
| actor_info                 | VIEW       |
| customer_list              | VIEW       |
| film_list                  | VIEW       |
| nicer_but_slower_film_list | VIEW       |
| sales_by_film_category     | VIEW       |
| sales_by_store             | VIEW       |
| staff_list                 | VIEW       |
+----------------------------+------------+
7 rows in set (0.00 sec)
```
*Aktuell sind 7 Views vorhanden*

Alle  Stored Procedures von der DB anzeigen:  
```sql
SHOW PROCEDURE STATUS WHERE db = 'sakila';
```
Ausgabe:  

```sql
+--------+-------------------+-----------+----------------+---------------------+---------------------+---------------+--------------------------------------------------+----------------------+----------------------+--------------------+
| Db     | Name              | Type      | Definer        | Modified            | Created             | Security_type | Comment                                          | character_set_client | collation_connection | Database Collation |
+--------+-------------------+-----------+----------------+---------------------+---------------------+---------------+--------------------------------------------------+----------------------+----------------------+--------------------+
| sakila | film_in_stock     | PROCEDURE | root@localhost | 2022-03-24 07:53:25 | 2022-03-24 07:53:25 | DEFINER       |                                                  | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| sakila | film_not_in_stock | PROCEDURE | root@localhost | 2022-03-24 07:53:25 | 2022-03-24 07:53:25 | DEFINER       |                                                  | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| sakila | rewards_report    | PROCEDURE | root@localhost | 2022-03-24 07:53:25 | 2022-03-24 07:53:25 | DEFINER       | Provides a customizable report on best customers | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
+--------+-------------------+-----------+----------------+---------------------+---------------------+---------------+--------------------------------------------------+----------------------+----------------------+--------------------+
3 rows in set (0.00 sec)
```
    
*Aktuell sind 3 Stored Procedures vorhanden*

Alle Triggers der DB anzeigen:  
```sql
show triggers in sakila;
``` 
Ausgabe:  

```sql
+----------------------+--------+----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------+------------------------+----------------------------------------------------------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
| Trigger              | Event  | Table    | Statement                                                                                                                                                                                                                                                                                                              | Timing | Created                | sql_mode                                                                                                                         | Definer        | character_set_client | collation_connection | Database Collation |
+----------------------+--------+----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------+------------------------+----------------------------------------------------------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
| customer_create_date | INSERT | customer | SET NEW.create_date = NOW()                                                                                                                                                                                                                                                                                            | BEFORE | 2022-03-24 07:53:36.08 | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | root@localhost | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| ins_film             | INSERT | film     | BEGIN
    INSERT INTO film_text (film_id, title, description)
        VALUES (new.film_id, new.title, new.description);
END                                                                                                                                                                                          | AFTER  | 2022-03-24 07:53:25.06 | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | root@localhost | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| upd_film             | UPDATE | film     | BEGIN
    IF (old.title != new.title) OR (old.description != new.description) OR (old.film_id != new.film_id)
    THEN
        UPDATE film_text
            SET title=new.title,
                description=new.description,
                film_id=new.film_id
        WHERE film_id=old.film_id;
    END IF;
END | AFTER  | 2022-03-24 07:53:25.09 | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | root@localhost | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| del_film             | DELETE | film     | BEGIN
    DELETE FROM film_text WHERE film_id = old.film_id;
END                                                                                                                                                                                                                                                     | AFTER  | 2022-03-24 07:53:25.11 | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | root@localhost | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| payment_date         | INSERT | payment  | SET NEW.payment_date = NOW()                                                                                                                                                                                                                                                                                           | BEFORE | 2022-03-24 07:53:37.59 | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | root@localhost | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
| rental_date          | INSERT | rental   | SET NEW.rental_date = NOW()                                                                                                                                                                                                                                                                                            | BEFORE | 2022-03-24 07:53:38.32 | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | root@localhost | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
+----------------------+--------+----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------+------------------------+----------------------------------------------------------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
6 rows in set (0.00 sec)
```
    
*Aktuell sind 6 Stored Procedures vorhanden*

## Auftrag Stored Procedures
Als Auftrag muss man den Create-Befehl der Stored Procedures film_in_stock dokumentieren.  

`film_in_stock` genauer beschrieben:  
```sql
| film_in_stock | STRICT_TRANS_TABLES,STRICT_ALL_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,TRADITIONAL,NO_ENGINE_SUBSTITUTION | CREATE DEFINER=`root`@`localhost` PROCEDURE `film_in_stock`(IN p_film_id INT, IN p_store_id INT, OUT p_film_count INT)
    READS SQL DATA
BEGIN
     SELECT inventory_id
     FROM inventory
     WHERE film_id = p_film_id
     AND store_id = p_store_id
     AND inventory_in_stock(inventory_id);

     SELECT COUNT(*)
     FROM inventory
     WHERE film_id = p_film_id
     AND store_id = p_store_id
     AND inventory_in_stock(inventory_id)
     INTO p_film_count;
END | utf8mb4              | utf8mb4_0900_ai_ci   | utf8mb4_0900_ai_ci |
```

Es gibt zwei int Eingabe Variablen (p_film_id und p_store_id). Die erste Variable wird für den Film ausgewählt, welcher zum Überprüfen angegeben wird.
Die zweite Eingabe ist für das Film Lager (Lager 1 oder 2). Die int Ausgabe p_film_count ist dann das schlussendliche Endresultat.  

In der oberen Zeilen wird von der `invetory` Datenbank die ID ausgwählt, wenn die `film_id` mit der `p_film_id` und die `store_id` mit der `p_store_id` aus der Übergabe stimmt.  

In der zeiten unteren Zeilen wird die Anzahl von der `inventory` tabelle genommen, wenn die `film_id` mit der `p_film_id` und die `store_id` mit der `p_store_id` aus der Übergabe stimmt. Dann wird das Resultat in die Variable `p_film_count` gespeichert.  

## Auftrag Views
Aufgabe:  
Dokumentieren Sie den Create-Befehl der View actor_info.  

Aufbau der View:  
```sql
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| View       | Create View                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | character_set_client | collation_connection |
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| actor_info | CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY INVOKER VIEW `actor_info` AS select `a`.`actor_id` AS `actor_id`,`a`.`first_name` AS `first_name`,`a`.`last_name` AS `last_name`,group_concat(distinct concat(`c`.`name`,': ',(select group_concat(`f`.`title` order by `f`.`title` ASC separator ', ') from ((`film` `f` join `film_category` `fc` on((`f`.`film_id` = `fc`.`film_id`))) join `film_actor` `fa` on((`f`.`film_id` = `fa`.`film_id`))) where ((`fc`.`category_id` = `c`.`category_id`) and (`fa`.`actor_id` = `a`.`actor_id`)))) order by `c`.`name` ASC separator '; ') AS `film_info` from (((`actor` `a` left join `film_actor` `fa` on((`a`.`actor_id` = `fa`.`actor_id`))) left join `film_category` `fc` on((`fa`.`film_id` = `fc`.`film_id`))) left join `category` `c` on((`fc`.`category_id` = `c`.`category_id`))) group by `a`.`actor_id`,`a`.`first_name`,`a`.`last_name` | utf8mb4              | utf8mb4_0900_ai_ci   |
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+

```

Der SQL Befehl etwas besser formatiert:  
```sql
 CREATE algorithm=undefined definer=`root`@`localhost` sql security invoker VIEW `actor_info` ASSELECT    `a`.`actor_id`   AS `actor_id`,
          `a`.`first_name` AS `first_name`,
          `a`.`last_name`  AS `last_name`,
          group_concat(DISTINCT concat(`c`.`NAME`,': ',
          (
                   SELECT   group_concat(`f`.`title` ORDER BY `f`.`title` ASC separator ', ')
                   FROM     ((`film` `f`
                   JOIN     `film_category` `fc`
                   ON      ((
                                              `f`.`` = `fc`.`film_id`)))
                   JOIN     `film_actor` `fa`
                   ON      ((
                                              `f`.`film_id` = `fa`.`film_id`)))
                   WHERE    ((
                                              `fc`.`category_id` = `c`.`category_id`)
                            AND      (
                                              `fa`.`actor_id` = `a`.`actor_id`)))) ORDER BY `c`.`NAME` ASC separator '; ') AS `film_info`
FROM      (((`actor` `a`
LEFT JOIN `film_actor` `fa`
ON       ((
                              `a`.`actor_id` = `fa`.`actor_id`)))
LEFT JOIN `film_category` `fc`
ON       ((
                              `fa`.`film_id` = `fc`.`film_id`)))
LEFT JOIN `category` `c`
ON       ((
                              `fc`.`category_id` = `c`.`category_id`)))
GROUP BY  `a`.`actor_id`,
          `a`.`first_name`,
          `a`.`last_name` 
```
In den ersten Schritten, werden auf die Spalten Referenz Variablen erstellt. Dann wird die Methode `group_concat` welche Bewirkt, dass alle Resultate in einzelne Datensätze angezeigt werden und nicht durch ein einzges ersetzt werden. Dabei wird eine Liste erstellt, von `category.name` welche mit `film.title` befüllt werden (Die Liste ist aufsteigen sortiert).  
Es wird ein inner join mit der film_category erstellt, wenn die `film_id` aus der Film Tablle mit der `film_id` aus der Film_categorie zusammenstimmt. Das selbe gilt auch für die `category_id` und die `actor_id` (Die Liste ist aufsteigend sortiert).  
Dann kommen 3 left joins dazu, für die Tabelle `film_actor`, `film_category` und `category`. Wenn die ids zueinander passen.  
Zum Schluss werden die aus der Tabelle `actor` die Spalten `a`.`actor_id`, `first_name` und`last_name` gruppiert.  
