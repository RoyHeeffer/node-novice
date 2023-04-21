# Opdracht - ExpressJs

> Navigeer naar de project sectie en lees de opdracht en start guidelines hoofdstukken. Je zult nog niet alle onderwerpen kennen, maar er word alvast een goed beeld geschetst van de requirements van de eind-applicatie.

## Korte uitleg

Zorg ervoor dat je stap 1 en 2 van de start guidelines hebt voldaan en lees wat er verwacht word in stap 3, hou hierbij ook rekening met de voorgestelde folder structuur uit deze sectie. In deze sectie werken we nog niet met de database, in plaats daarvan maken we voor nu een `database.ts`(Deze vervangen we op een later punt voor een echte database).
Creëer een **database.ts** file in de **src** folder, en laat deze twee **object-arrays** exporteren, 1 waar de users worden bijgehouden en 1 voor de posts(Zoals gezegd benoem deze naar eigen inzicht), vul deze beide met een test-data object.

Hieronder een klein voorbeeld:

```javascript
/* database.ts */

export const Users = [
    {
        id: 1
        username: 'Test',
        email: 'test@test.nl',
        password: 'Test',
    }
];
export const Posts = [
    {
        id: 1,
        userId: 1,
        title: "My very first blog",
        content: "Lorem ipsum...",
    }
];

```

## Routes

Maak alvast de volgende routes aan in je applicatie, zorg ervoor dat de gebruikte benamingen overeenkomen met jouw database-objecten:

#### User routes

**POST** /users:
Deze route maakt een nieuwe gebruiker aan en vereist de volgende parameters in het body object:

- **username**: de gebruikersnaam van de nieuwe gebruiker.
- **email**: de email van de nieuwe gebruiker, deze is uniek per gebruiker.
- **password**: het wachtwoord van de nieuwe gebruiker.

Als het emailadres nog niet bestaat wordt er een nieuwe gebruiker aangemaakt en wordt er een JWT-token geretourneerd, bestaat deze al wel geef dan een passende response.

**POST** /users/login:
Deze route logt een gebruiker in en vereist de volgende parameters in het body object:

- **username**: de gebruikersnaam van de gebruiker.
- **password**: het wachtwoord van de gebruiker.

Als de combinatie van gebruikersnaam en wachtwoord correct is, wordt er een JWT-token geretourneerd.

**GET** /users/:id/posts:
Deze route haalt alle posts op van een gebruiker met de gegeven id.

Er moet een geldig JWT-token in de Authorization header worden meegegeven.

#### Post routes

**GET** /posts:
Deze route haalt alle posts op van alle gebruikers.

Er moet een geldig JWT-token in de Authorization header worden meegegeven.

**POST** /posts:
Deze route maakt een nieuwe post aan en vereist de volgende parameters in het body object:

- **title**: de titel van de post.
- **content**: de inhoud van de post.

Er moet een geldig JWT-token in de Authorization header worden meegegeven. De nieuwe post wordt aangemaakt en de id van de post wordt geretourneerd.

**PUT** /posts/:id:
Deze route update de post met het gegeven id en vereist de volgende parameters in het body object:

- **title**: de nieuwe titel van de post
- **content**: de nieuwe inhoud van de post

Er moet een geldig JWT-token in de Authorization header worden meegegeven. De post wordt alleen bijgewerkt als de gebruiker die de post heeft aangemaakt overeenkomt met de gebruiker in de JWT-token.

**DELETE** /posts/:id:
Deze route verwijdert de post met het gegeven id.

Er moet een geldig JWT-token in de Authorization header worden meegegeven. De post wordt alleen verwijderd als de gebruiker die de post heeft aangemaakt overeenkomt met de gebruiker in de JWT-token.

#### Middleware

> Bekijk in de project sectie het hoofdstuk helpers, hier vind je voorbeelden hoe er simpele middleware kan worden opgezet(Let op deze voorbeelden gaan uit van een TypeORM connectie, je moet ze nu eerst aan je database.ts file koppelen)

**Authenticate**<br/>
Deze middleware controleert of er een geldig JWT-token in de Authorization header wordt meegegeven en of de gebruiker bestaat. Als er geen geldig token wordt meegegeven, wordt er een 401 Unauthorized error geretourneerd.

Als de token geldig is en de gebruiker bestaat, wordt het user object toegevoegd aan de request. De volgende routes vereisen de Authenticate middleware:

- GET /users/:id/posts
- GET /posts
- POST /posts
- PUT /posts/:id
- DELETE /posts/:id

**Authorize**<Br/>
Deze middleware controleert of de gebruiker die een bepaalde actie uitvoert overeenkomt met de gebruiker in de JWT-token. Als dit niet het geval is, wordt er een 401 Unauthorized error geretourneerd. De volgende routes vereisen de Authorize middleware:

- PUT /posts/:id
- DELETE
