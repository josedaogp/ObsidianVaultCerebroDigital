## Am-E
```dataview
list
from "Hobbies_y_Pasatiempos/Guitarra/Flamenco"
where rueda_acordes = "Am-E"

```
## E-F
```dataview
list
from "Hobbies_y_Pasatiempos/Guitarra/Flamenco"
where rueda_acordes = "E-F"

```

## Em-B
```dataview
list
from "Hobbies_y_Pasatiempos/Guitarra/Flamenco"
where rueda_acordes = "Em-B"

```
## A-Bb
```dataview
list
from "Hobbies_y_Pasatiempos/Guitarra/Flamenco"
where rueda_acordes = "A-Bb"

```
## Resumen todas las ruedas que tengo
```dataview
table length(rows) as "Número de Notas", rueda_acordes as "Ruedas de Acordes"
from "Hobbies_y_Pasatiempos/Guitarra/Flamenco"  
where rueda_acordes
group by rueda_acordes

```

## Sevillanas y su rueda
```dataview
table rueda_acordes as "Rueda de Acordes"
from "Hobbies_y_Pasatiempos/Guitarra/Flamenco"  
where rueda_acordes
```
