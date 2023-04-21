# Start guidelines

## Over de app

Je gaat een eenvoudige blog-app bouwen, Het project heeft twee belangrijke entities: `Post` en `User`. De User entity bevat informatie over de gebruikers die de blogposts kunnen aanmaken(een User kan een auteur zijn). De Post entity bevat informatie over de blogposts, zoals de titel, de inhoud en de auteur. Bedenk alvast welke functionaliteit deze moet hebben en hoe je hier het beste op in mee om gaat.

## Verwachting

De blog-applicatie moet aan de volgende voorwaarden voldoen:

- Een user moet kunnen worden aangemaakt.
- Een user moet kunnen inloggen.
- Alle posts moeten kunnen worden opgehaald.
- Er moet een nieuwe post kunnen worden aangemaakt.
- Een post moet kunnen worden bijgewerkt(Alleen eigen posts).
- Een post moet ook verwijderd kunnen worden(Alleen eigen posts).

## Quick-start tips

**1**: Installeer Node.js en NPM
Voordat je begint, zorg ervoor dat je Node.js en NPM hebt geïnstalleerd op je computer. Je kunt de nieuwste versie downloaden van de officiële [Node.js-website].

**2**: Installeer de benodigde dependencies voor de blogapplicatie met het command `npm install express cors ejs bcrypt jsonwebtoken knex typeorm mysql2`. Dit installeert de Express.js webframework, CORS, bcrypt voor wachtwoordversleuteling, jsonwebtoken voor authenticatie, Knex voor migrations, TypeORM als ORM en MySQL2 als database driver.

**3**:Maak de endpoints aan voor het maken, inloggen en beheren van gebruikers en het bekijken, toevoegen, wijzigen en verwijderen van blogposts. Gebruik Express.js om de endpoints aan te maken, op dit punt hebben we nog geen connectie met de database.

**4**:Maak de migration-files voor Post en User aan met de properties zoals eerder beschreven(Dit is een tip, je bent vrij je entities te noemen zoals je wilt), maak ook seeders aan om de database te vullen met testdata.

**5**:Maak de TypeORM entiteiten Post en User aan met de properties zoals eerder beschreven(Benoem ze in camel-case, gebruik dezelfde benamingen als die in de migrations-files). Je kunt na de TypeORM config opzet TypeORM gebruiken om de entiteiten aan te maken en te koppelen aan de MySQL-database.

[node.js-website]: https://nodejs.org/en
