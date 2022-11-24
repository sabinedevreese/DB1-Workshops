# 06 - Workshop SQL : Imdb

## Diagram
<p align="center">
    <img width="75%"  src="../../images/diagram-imdb.jpg" alt="Diagram Imdb.db">
</p>

> Meer informatie over de databank kan je [hier](https://www.imdb.com/interfaces/) vinden.

## Oplossingen 1 - Raadplegen
1. Geef de unieke lijst van genres. Sorteer alfabetisch. (27 recrords)
```sql
SELECT DISTINCT genre FROM genres
ORDER BY genre
```
2. Geef de unieke lijst van tvMiniSeries die zijn opgenomen in titles. (35 records)
```sql
SELECT * FROM titles
WHERE titleType = 'tvMiniSeries'
```
3. Welke verschillende titleType's zijn er opgenomen in titles. Sorteer alfabetisch. (4 records)
```sql
SELECT DISTINCT titleType
FROM titles
ORDER BY titleType ASC
```
4. Geef alle films uit het jaar 2010 tot en met 2020. (1318 records)
```sql
SELECT *
FROM titles
WHERE titleType = 'movie' and startYear BETWEEN 2010 and 2020
```
5. Geef de primaryTitle van alle tvMiniSeries die meer dan 1 jaar liepen (startYear < endYear). (3 records)
```sql
SELECT *
FROM titles
WHERE titleType = 'tvMiniSeries' AND endYear > startYear
```
6. Geef de personen met de naam Clooney. (2 records)
```sql
SELECT *
FROM persons
WHERE fullName LIKE '%Clooney%'
```
7. Geef alle persons die jonger dan 50 jaar zijn. (in 2022: 1437 records)
```sql
SELECT *
FROM persons
WHERE birthYear > YEAR(NOW()) - 50
```
8. Zijn er persons die 100 jaar of ouder zijn en die nog in leven zijn (volgens de informatie in de databank)? (in 2022: 3 records)
```sql
SELECT *
FROM persons
WHERE birthYear <= YEAR(NOW()) - 100 AND deathYear IS NULL
```
9. Geef de namen van de films die beginnen ‘The’. (921 records)
```sql
SELECT *
FROM titles
WHERE titleType = 'movie' AND primaryTitle LIKE 'The%'
```
10. Geef alle Pirates of the Carribean films, maar je twijfelt over de schrijfwijze. Hoe los je dat op? (4 records)
```sql
SELECT *
FROM titles
WHERE titleType = 'movie' AND primaryTitle LIKE '%pirates of the %'
```
11. Geef het verschijningsjaar van je favoriete film.
12. In welk jaar kwam de film 'The Hangover II' uit? (2013)
```sql
SELECT primaryTitle, startYear
FROM titles
WHERE primaryTitle LIKE '%hangover%'
```
13. Geef lijst van de namen van personen aflopend gesorteerd op birthYear. In geval van gelijk birthYear, wordt er oplopend gesorteerd op fullName
```sql
SELECT fullName, birthYear
FROM persons
ORDER BY birthYear DESC, fullName ASC
```
14. Geef de langst lopende tvSerie waarbij endYear is ingevuld. (The X files --> 25 jaar)
```sql
SELECT primaryTitle, endYear - startYear
FROM titles
WHERE endYear IS NOT NULL
ORDER BY 2 DESC
LIMIT 1
```
15. Geef de 10 langste films uit de databank. Schrijf dit als uren en minuten. (Gangs of Wasseypur 5 uur 21 min, ...)
```sql
SELECT primaryTitle, CONCAT(FLOOR(runtimeMinutes / 60), 'uur ', MOD(runtimeMinutes, 60), 'min')
FROM titles
WHERE titleType = 'movie'
ORDER BY runtimeMinutes DESC
LIMIT 10
```
16. Zijn er films waarbij endYear wel is ingevuld? (Neen)
```sql
SELECT *
FROM titles
WHERE titleType = 'movie' AND endYear IS NOT NULL
```
17. Geef de primaryTitles van de isAdult films aanwezig in de databank. (Er zijn geen isAdult films in de databank)
```sql
SELECT *
FROM titles
WHERE titleType = 'movie'
WHERE isAdult <> 0
```
18. Geef een overzicht van primaryTitles, runtimeMinutes en een opmerking als volgt
  - runtimeMinutes < 60: Short movie
  - runtimeMinutes tussen 60 en 150: Normal movie
  - runtimeMinutes tussen 151 en 210: Long movie
  - runtimeMinutes > 210: Extreme long movie
  (Eerste film is Sherlock Jr. + 45 minuten + Short movie)
```sql
SELECT primaryTitle, runtimeMinutes,
CASE
	WHEN runtimeMinutes < 60 THEN 'Short movie'
    WHEN runtimeMinutes BETWEEN 60 AND 150 THEN 'Normal movie'
    WHEN runtimeMinutes BETWEEN 151 AND 210 THEN 'Long movie'
    ELSE 'Extreme long movie'
END As 'Movie Duration'
FROM titles
```

## Oefeningen
Klik [hier](../../exercises.md) om terug te gaan naar de oefeningen.
