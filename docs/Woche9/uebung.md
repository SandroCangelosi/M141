# Übung Performance
Als erstes musste ich eine Datei mit dem Skript Inhalt von Moodle erstellen und importieren.  
```bash
sudo mysql < import_performance.sql 
```

Mit diesem Befehl testen wir den SELECT Befehl:  
```sql
EXPLAIN SELECT * FROM orderdetails d     
    INNER JOIN orders o ON d.orderNumber = o.orderNumber    
    INNER JOIN products p ON p.productCode = d.productCode     
    INNER JOIN productlines l ON p.productLine = l.productLine     
    INNER JOIN customers c on c.customerNumber = o.customerNumber      
WHERE o.orderNumber = "10101";
```

Schlechte Ausgabe:  
```sql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                              |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
|  1 | SIMPLE      | l     | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    7 |   100.00 | NULL                                               |
|  1 | SIMPLE      | p     | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  110 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
|  1 | SIMPLE      | o     | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  326 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
|  1 | SIMPLE      | c     | NULL       | ALL  | NULL          | NULL | NULL    | NULL |  122 |    10.00 | Using where; Using join buffer (Block Nested Loop) |
|  1 | SIMPLE      | d     | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 2996 |     1.00 | Using where; Using join buffer (Block Nested Loop) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------------------------------------------+
5 rows in set, 1 warning (0.00 sec)
```
*Es werden alle Spalten multipliziert, dann kommt man auf die Anzahl gescannter Elemente: 7 x 110 x 326 x 122 x 2996 = 91 750 822 240*

Jetzt haben wir noch auf die Tabellen Primary und Foreign Keys hinzugefügt:  
```sql
ALTER TABLE customers ADD PRIMARY KEY (customerNumber);
ALTER TABLE employees ADD PRIMARY KEY (employeeNumber);
ALTER TABLE offices ADD PRIMARY KEY (officeCode);
ALTER TABLE orderdetails ADD PRIMARY KEY (orderNumber, productCode);
ALTER TABLE orders ADD PRIMARY KEY (orderNumber), ADD KEY (customerNumber);
ALTER TABLE payments ADD PRIMARY KEY (customerNumber, checkNumber);
ALTER TABLE productlines ADD PRIMARY KEY (productLine);
ALTER TABLE products ADD PRIMARY KEY (productCode), ADD KEY (buyPrice), ADD KEY (productLine);
ALTER TABLE productvariants ADD PRIMARY KEY (variantId),ADD KEY (buyPrice),ADD KEY (productCode);
```

Dann sieht die Ausgabe schon besser aus:  
```sql
+----+-------------+-------+------------+--------+------------------------+---------+---------+------------------------------+------+----------+-----------------------+
| id | select_type | table | partitions | type   | possible_keys          | key     | key_len | ref                          | rows | filtered | Extra                 |
+----+-------------+-------+------------+--------+------------------------+---------+---------+------------------------------+------+----------+-----------------------+
|  1 | SIMPLE      | o     | NULL       | const  | PRIMARY,customerNumber | PRIMARY | 4       | const                        |    1 |   100.00 | NULL                  |
|  1 | SIMPLE      | c     | NULL       | const  | PRIMARY                | PRIMARY | 4       | const                        |    1 |   100.00 | NULL                  |
|  1 | SIMPLE      | d     | NULL       | ref    | PRIMARY                | PRIMARY | 4       | const                        |    4 |   100.00 | Using index condition |
|  1 | SIMPLE      | p     | NULL       | eq_ref | PRIMARY,productLine    | PRIMARY | 17      | explaintesting.d.productCode |    1 |   100.00 | NULL                  |
|  1 | SIMPLE      | l     | NULL       | eq_ref | PRIMARY                | PRIMARY | 52      | explaintesting.p.productLine |    1 |   100.00 | NULL                  |
+----+-------------+-------+------------+--------+------------------------+---------+---------+------------------------------+------+----------+-----------------------+
5 rows in set, 1 warning (0.00 sec)
```
*Gesante Elemente: 1 x 1 x 4 x 1 x 1 = 4*