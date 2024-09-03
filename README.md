#### Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)

```sql
SELECT *
FROM `events`
WHERE `price` IS NULL;
```

#### Selezionare tutte le location in ordine alfabetico (82)

```sql
SELECT *
FROM `locations`
ORDER BY `name` ASC;
```

#### Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)

```sql
SELECT *
FROM `events`
WHERE `price` < 20 AND HOUR(`duration`) < 3;
```

#### Selezionare tutti gli eventi di dicembre 2023 (25)

```sql
SELECT *
FROM `events`
WHERE YEAR(`start`) = 2023 AND  MONTH(`start`) = 12;
```

#### Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)

```sql
SELECT *
FROM `events`
WHERE HOUR(`duration`) > 2;
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

```sql
SELECT *
FROM `events`
WHERE `user_id` = 1202;
```

#### Selezionare il numero totale di eventi per ogni fascia di prezzo (81)

```sql
SELECT *
FROM `events`
GROUP BY `price`
```

#### Selezionare tutti gli utenti admin ed editor (9)

```sql
SELECT *
FROM `users`
WHERE `role_id` = 1 OR `role_id` = 2;
```

#### Selezionare tutti i concerti (eventi con il tag “concerti”) (72)

```sql 
SELECT `events`.`name`,`events`.`description`
FROM `events` 
INNER JOIN `event_tag` ON `events`.`id` = `event_tag`.`event_id`
INNER JOIN `tags` ON `event_tag`.`tag_id` = `tags`.`id`
WHERE `tags`.`name` = "Concerti";
```
OR
```sql 
SELECT `events`.`name`,`events`.`description`
FROM `events` 
INNER JOIN `event_tag` ON `events`.`id` = `event_tag`.`event_id`
INNER JOIN `tags` ON `event_tag`.`tag_id` = `tags`.`id`
WHERE `event_tag`.`tag_id` = 1;
```

#### Selezionare tutti i tag e il prezzo medio degli eventi a loro collegati (11)

```sql 
SELECT `tags`.`name` as nome_tag, 
        AVG(`events`.`price`) as prezzo_medio
FROM `events` 
INNER JOIN `event_tag` ON `events`.`id` = `event_tag`.`event_id`
INNER JOIN `tags` ON `event_tag`.`tag_id` = `tags`.`id`
GROUP BY `tags`.`name`;
```

#### Selezionare tutte le location e mostrare quanti eventi si sono tenute in ognuna di esse (82)

```sql 
SELECT
    `events`.`location_id`,
    COUNT(*) AS numero_eventi
FROM
    `events`
GROUP BY
    `events`.`location_id`;
```

#### Selezionare tutti i partecipanti per l’evento “Concerto Classico Serale” (slug: concerto-classico-serale, id: 34) (30)

```sql 
SELECT `users`.`id`, `users`.`first_name`,`users`.`last_name` 
FROM `users` 
INNER JOIN `bookings` ON `users`.`id` = `bookings`.`user_id`
WHERE `bookings`.`event_id` = 34;
```

#### Selezionare tutti i partecipanti all’evento “Festival Jazz Estivo” (slug: festival-jazz-estivo, id: 2) specificando nome e cognome (13)

```sql 
SELECT 
    `users`.`first_name` AS nome, 
    `users`.`last_name` AS cognome
FROM 
    `users`
INNER JOIN `bookings` ON `users`.`id` = `bookings`.`user_id`
INNER JOIN `events` ON `bookings`.`event_id` = `events`.`id`
WHERE `events`.`slug` = "festival-jazz-estivo"
```

#### Selezionare tutti gli eventi sold out (dove il totale delle prenotazioni è uguale ai biglietti totali per l’evento) (18)

```sql
SELECT 
    `events`.`id` AS id_evento, 
    `events`.`name` AS nome_evento,
    `events`.`total_tickets`,
    COUNT(`bookings`.`id`) AS prenotazioni
FROM 
    `events`
# left join per includere tutti gli eventi
LEFT JOIN 
    `bookings` ON  `events`.`id` = `bookings`.`event_id`
GROUP BY 
    `events`.`id`
HAVING 
    `events`.`total_tickets` = prenotazioni;
```


#### Selezionare tutte le location in ordine per chi ha ospitato più eventi (82)

```sql 
SELECT `locations`.*, COUNT(`events`.`id`) AS numero_eventi
FROM `locations`
LEFT JOIN `events` ON `events`.`location_id` = `locations`.`id`
GROUP BY `locations`.`id`
ORDER BY numero_eventi DESC;
```
#### Selezionare tutti gli utenti che si sono prenotati a più di 70 eventi (74)
```sql
SELECT `users`.*, COUNT(`bookings`.`user_id`) AS prenotazioni
FROM `users`
LEFT JOIN `bookings` ON `bookings`.`user_id` = `users`.`id`
GROUP BY `users`.`id`
HAVING prenotazioni > 70
```

#### Selezionare tutti gli eventi, mostrando il nome dell’evento, il nome della location, il numero di prenotazioni e il totale di biglietti ancora disponibili per l’evento (1040)

```sql
SELECT 
    `events`.`name` AS nome_evento,
    `locations`.`name` AS location,
    COUNT(`bookings`.`id`) AS numero_prenotazioni,
    `events`.`total_tickets` - COUNT(`bookings`.`id`) AS biglietti_disponibili
FROM 
    `events`
JOIN 
    `locations` ON `events`.`location_id` = `locations`.`id`
LEFT JOIN 
    `bookings` ON `bookings`.`event_id` = `events`.`id`
GROUP BY 
    `events`.`id`, `events`.`name`, `locations`.`id`, `locations`.`name`, `events`.`total_tickets`
ORDER BY 
    `events`.`name`;
```