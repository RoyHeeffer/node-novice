# Seed data invoeren met Knex.js

Om seed data in onze database in te voeren met Knex.js, moeten we de volgende stappen uitvoeren:

## Maak een seed file aan

Een seed file is een JavaScript-bestand waarin de seed data wordt gedefinieerd die we willen invoeren in onze database. De seed file moet worden opgeslagen in de `/seeds` map van ons project.
Voer de seed data in: Om de seed data in onze database in te voeren, gebruiken we het commando knex seed:run in de terminal. Dit commando zal de code uitvoeren die we in onze seed file hebben geschreven en de data invoeren in onze database.
Laten we eens kijken hoe we deze stappen kunnen uitvoeren in ons project.

**Stap 1**: Maak een seed file aan
Om een seed file aan te maken, moeten we het knex seed:make commando gebruiken in de terminal. Dit commando zal een nieuw JavaScript-bestand aanmaken in de /seeds map van ons project.

Laten we eens kijken naar een voorbeeld van hoe we een seed file kunnen aanmaken voor onze posts tabel:

```sh
knex seed:make seed_posts

```

Dit commando zal een nieuw JavaScript-bestand aanmaken genaamd seed_posts.js in de /seeds map van ons project.

Laten we vervolgens de inhoud van ons seed_posts.js bestand bekijken:

```javascript
exports.seed = function (knex) {
  // Deletes ALL existing entries
  return knex("posts")
    .del()
    .then(function () {
      // Inserts seed entries
      return knex("posts").insert([
        {
          title: "First post",
          content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
          author: "John Doe",
        },
        {
          title: "Second post",
          content:
            "Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          author: "Jane Smith",
        },
        {
          title: "Third post",
          content:
            "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
          author: "Bob Johnson",
        },
      ]);
    });
};
```

In dit voorbeeld hebben we een seed file gemaakt genaamd seed_posts.js die drie vooraf gedefinieerde posts invoert in onze database.

**Stap 2**: Voer de seed data in
Nu we onze seed file hebben gemaakt, kunnen we de seed data invoeren in onze database. Om de database te vullen met data, kun je een seed-bestand maken in de seeds-map van je project. Het seed-bestand moet een functie exporteren die een knex object als parameter accepteert. Met behulp van het knex object kun je de gewenste gegevens toevoegen aan de database. Hier is een voorbeeld van een eenvoudige seed-functie:

```javascript
exports.seed = function (knex) {
  return knex("users")
    .del()
    .then(function () {
      return knex("users").insert([
        {
          id: 1,
          name: "John Doe",
          email: "john@example.com",
          password: "password123",
        },
        {
          id: 2,
          name: "Jane Doe",
          email: "jane@example.com",
          password: "password456",
        },
      ]);
    });
};
```

In dit voorbeeld wordt de users tabel leeggemaakt en vervolgens worden er twee rijen met gegevens toegevoegd aan de tabel.

Je kunt het seed-bestand uitvoeren met het volgende Knex-commando:

```sh
npx knex seed:run
```

Dit zal de seed-functie uitvoeren en de gegevens in de database toevoegen.
