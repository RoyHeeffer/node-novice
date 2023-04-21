# Opdracht - KnexJs

> Bekijk nogmaals de opdracht in de project sectie en zet de database op, er is een dockerfile met instructies te vinden in de helper sectie.

## Korte uitleg

We gaan nu beginnen met **stap 4** uit de start guidelines, lees goed wat er verwacht wordt. Hou hierbij nogmaals rekening met de voorgestelde folder structuur uit deze sectie. In deze sectie werken gaan we werken met een database, zorg ervoor dat je deze hebt opgezet.

## Configuratie

Zet de configuratie op om Knex.js met je database te connecten, bekijk hiervoor nogmaals het install hoofdstuk.

## Migration-bestanden

Migration-bestanden zijn scripts die worden gebruikt om het database-schema bij te werken. Ze beschrijven de veranderingen die moeten worden aangebracht in de database, zoals het aanmaken van nieuwe tabellen of het wijzigen van de bestaande. Migraties zijn handig omdat ze de developer in staat stellen om wijzigingen in het schema te beheren en versiebeheer overzicht te hebben. Maak de migration-bestanden aan die je applicatie nodig heeft, denk hierbij goed na over wat iedere tabel moet bijhouden aan data:

Om een migratiebestand te maken, voert je het volgende command uit in de terminal:

```bash
npm run knex:migrate:make migration_name
```

Dit maakt een nieuw migratiebestand met de naam migration_name.ts in de migrations map,
maak migrations aan voor alle benodigde database tabellen en hun relaties.

## Seeder-bestanden

Seeder-bestanden worden gebruikt om testgegevens in de database te plaatsen. Ze worden vaak gebruikt om de database te vullen met testgegevens of met gegevens die nodig zijn om een applicatie uit te voeren. Seeder-bestanden zijn handig omdat ze het proces van het plaatsen van gegevens in de database automatiseren. Maak wat seeder-bestanden aan om je database van test-data te voorzien:

Om een seederbestand te maken, voert je het volgende command uit in de terminal:

```bash
npm run knex:seed:make migration_name
```

Dit maakt een nieuw seederbestand met de naam seed_name.ts in de seeds map, In de seed functie voeg je de gegevens in de database in.

**Het uitvoeren van migraties en seeders**<br/>
Om migraties en seeders uit te voeren, voert je de volgende command's uit in de terminal:

```bash
npm run knex:migrate:latest
npm run knex:seed:run
```

Het eerste command voert alle nog niet uitgevoerde migraties uit. Het tweede command voert alle nog niet uitgevoerde seeders uit.

> Bekijk in je database GUI of de nieuwe migrations goed zijn doorgevoerd, je zou nu de tabellen moeten kunnen zien in je database!
