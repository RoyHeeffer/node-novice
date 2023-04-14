# Seed data invoeren met Knex.js

Om seed data in onze database in te voeren met Knex.js, moeten we de volgende stappen uitvoeren:

## Maak een seed file aan

Een seed file is een JavaScript-bestand waarin de seed data wordt gedefinieerd die we willen invoeren in onze database. De seed file moet worden opgeslagen in de `/seeds` map van ons project.

**Stap 1**: Om een seed file aan te maken, moeten we het knex seed:make command gebruiken in de terminal. Dit command zal een nieuw JavaScript-bestand aanmaken in de **/seeds map** van ons project.

```sh
npx knex seed:make seed_users

```

Dit commando zal een nieuw JavaScript-bestand aanmaken genaamd `seed_users.js` in de /seeds map van ons project.

Een voorbeeld van een seeder file zou er als volgt uitzien:

```javascript
import { Knex } from "knex";

export async function seed(knex: Knex): Promise<void> {
  // Deletes ALL existing entries
  await knex("users").del();

  // Inserts seed entries
  await knex("users").insert([
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
}
```

In dit voorbeeld wordt de users tabel leeggemaakt en vervolgens worden er twee rijen met gegevens toegevoegd aan de tabel.

**Stap 2**: Nu we onze seed file hebben gemaakt, kunnen we de seed data invoeren in onze database. Het seed-bestand exporteerd een functie die een knex object als parameter accepteert. Met behulp van het knex object kun je de gewenste gegevens toevoegen aan de database.

Je kunt het seed-bestand uitvoeren met het volgende Knex-command:

```sh
npx knex seed:run
```

Dit zal de seed-functie uitvoeren en de gegevens in de database toevoegen.

## Factories

Knex.js kent ook factories voor snellere data seeding, om factories te gebruiken moet je een package zoals `knex-factory` installeren voor. Run voor de factories lib het volgende command in je terminal:

```sh
npm i knex-factory faker
```

Vervolgens kun je een factory-definitiebestand maken, waarin je de eigenschappen van de objecten definieert die je wilt genereren.

Hier is een eenvoudig voorbeeld van een factory-definitie voor een "users" tabel:

```javascript
import knex from "knex";
import { Factory } from "knex-factory";
import faker from "faker";

Factory.define("users", async () => {
  return {
    name: faker.name.firstName(),
    email: faker.internet.email(),
    password: await bcrypt.hash("password", 10),
    created_at: new Date(),
    updated_at: new Date(),
  };
});

// Seed code ...
```

In dit voorbeeld definieert de factory een object met eigenschappen zoals naam, e-mailadres en wachtwoord. De `faker` module wordt gebruikt om realistische namen en e-mailadressen te genereren. Merk op dat we ook de bcrypt-module gebruiken om het wachtwoord te hashen voordat we het opslaan in de database.

Met de factory-definitie kunnen we nu eenvoudig objecten genereren met de gewenste eigenschappen. Bijvoorbeeld:

```javascript
import { Knex } from "knex";

// Factory code...

export async function seed(knex: Knex): Promise<void> {
  // Deletes ALL existing entries
  await knex("users").del();

  const user = await Factory.create("users");
  // Inserts seed entries
  await knex("users").insert(user);
}
```

Dit genereert een object met willekeurige gegevens en slaat deze op in de database. Je kunt ook meerdere objecten genereren met de `createMany`-functie:

```javascript
const users = await Factory.createMany("users", 5);
```

Dit genereert vijf objecten en slaat ze op in de database.

## Nog een voorbeeld

In dit voorbeeld hebben we een seed file gemaakt genaamd seed_posts.js die drie vooraf gedefinieerde posts invoert in onze database, deze keer hier maken we gebruik van **function chaining**. Beide manieren zijn goed, het is een kwestie van voorkeur wat je gebruikt, zolang je maar consistent die optie blijft gebruiken.

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
