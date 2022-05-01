# Javascript in MongoDB
Durch die MongoDB Shell kann man in MongoDB Javascript verwenden.  
So hat man die Möglichkeit Javascript Befehle auszuführen.  

## typeof
Da in Javascript der Typ einer Variablen nicht ausdrücklich deklariert wird kann mit diesem Operator festgestellt werden was für ein Datentyp automatisch ausgewählt wurde.  

Beispiel in MongoDB:  
![Typeof Beispiel in MongoDB](typeof.png)  
*Mögliche Ausgaben sind: Undefined, Null, Boolean, String, Symbol, Number und Object.*  

## Funtionen
Durch Javascript kann man auch Funtionen erstellen und diese Ausführen:  

Erstellung:  
```javascript
function insertCity(
          name, population, lastCensus,
          famousFor, mayorInfo
        ) {
          db.towns.insert({
            name: name,
            population: population,
            lastCensus: ISODate(lastCensus),
            famousFor: famousFor,
            mayor : mayorInfo
          });
        }
```

Ausführung:  
```javascript
insertCity("Weinfelden", 11588, '2022-02-22',
    ["Jacks", "BBZ", "Model"], { name : "Max Vögeli", party: "FDP"}
)
```

## Vergleichen von Werten
Wenn man Variablen auf etwas Vergleichen möchte, kann dies mit Schlüsselwörtern oder REGEX geschehen.  

Liste von Schlüsselwörtern:  

Kommando | Beschriebung
-------- | --------
$regex 	| Überprüft, ob Reguläre Ausdrücke werden verwendet werden
$ne 	| Nicht gleich
$lt 	| Weniger als
$lte 	| Weniger als oder gleich
$gt 	| Grösser als
$gte 	| Grösser als oder gleich
$exists | Überprüft, ob ein Felx existiert
$all 	| Überprüft, ob es zu allen Elementen im Array passt
$in 	| Überprüft, ob es zu einem Elementen im Array passt
$nin 	| Überprüft, ob es zu keinem Elementen im Array passt
$elemMatch| Überprüft, ob es zu allen Elementen im verschalteltem Array passt
$or 	| oder
$nor 	| Nicht oder
$size 	| Überprüft, ob der Array zu einer gegebenen Grösse passt
$mod 	| Gibt den Restwert zurück, der übrig bleibt wenn ein Operand durch einen anderen geteilt wird
$type 	| Überprüft, ob der Array zu einer gegebenen Datentyp passt
$not 	| Nicht gleich


Hier ein Beispiel:  
```javascript
> db.towns.find( { 
    name: /^F/, 
    population : { $lt : 30000 } 
    }, 
    { _id: 0, 
    name : 1, 
    population : 1 }
    )
{ "name" : "Frauenfeld", "population" : 25974 }
> 
```
*Der Name muss mit "F" beginnen (Regex)*  
*Die Population muss kleiner als 30000 sein(Schlüsselwort)*  