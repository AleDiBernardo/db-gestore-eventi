#### Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)

```sql
SELECT * FROM `events` WHERE `price` IS NULL;
```

#### Selezionare tutte le location in ordine alfabetico (82)

```sql
SELECT * FROM `locations` ORDER BY `name` ASC;
```

#### Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)

```sql
SELECT * FROM `events` WHERE `price` < 20 AND HOUR(`duration`) < 3;
```

#### Selezionare tutti gli eventi di dicembre 2023 (25)

```sql
SELECT * FROM `events` WHERE YEAR(`start`) = 2023 AND  MONTH(`start`) = 12;
```

#### Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)

```sql
SELECT * FROM `events` WHERE HOUR(`duration`) > 2;
```

#### Selezionare tutti gli eventi, mostrando nome, data di inizio, ora di inizio, ora di fine e durata totale (1040)

```sql
SELECT `events`.`name`, DATE(`events`.`start`) as data_inizio,
	CONCAT(HOUR(`events`.`start`), ':',LPAD(MINUTE(`events`.`start`),2,'0')) as ora_inizio,
    CONCAT(LPAD(HOUR(ADDTIME(`events`.`start`,`events`.`duration`)),2,'0'), ':', LPAD(MINUTE(ADDTIME(`events`.`start`,`events`.`duration`)),2,'0')) as ora_fine,
    `events`.`duration` as durata_totale
FROM `events`;
```

#### Selezionare tutti gli eventi aggiunti da “Fabiano Lombardo” (id: 1202) (132)
