# Installeren KnexJs

Knex.js is een database-toolkit voor JavaScript die het uitvoeren van database-operaties, zoals het creëren van tabellen en het uitvoeren van migraties, vergemakkelijkt. Hieronder staan de stappen voor het installeren, configureren en instellen van de folderstructuur van Knex.js.

Installeer Knex.js via npm door het volgende commando uit te voeren in de terminal:

```bash
npm install knex
```

## Configureren

Maak een knexfile.js-bestand aan en zet deze in een nieuwe folder genaamd **db**  in de **src-folder** van je project en zet de volgende code uit om een basisconfiguratie in te stellen:

```javascript
// knexfile.js

module.exports = {
  development: {
    client: "sqlite3",
    connection: {
      filename: "./dev.sqlite3"
    }
  },
  production: {
    client: "postgresql",
    connection: {
      database: "my_db",
      user: "username",
      password: "password"
    }
  }
};
```

Nu de configuratie is opgezet gaan we een aantal commands aan onze package.json toevoegen, deze zullen we later weer gebruiken in de migrations en seeders secties.

Als laatste dependency voegen we **ts-node-dev** toe, deze package herstart de applicatie wanneer er veranderingen in de folder worden geconstateerd(**Hot-module reloading**):

```bash
npm install -D ts-node-dev
```

Voeg nu in `package.json` onder de key **scripts** het volgende command toe:

```json
"scripts": {
    "dev": "ts-node-dev --respawn --transpile-only --exit-child --watch src src/app.ts",
    // Other scripts...
  }
```



## Aanvulling in folderstructuur 

De folder structuur voor je Express.js-applicatie zou er nu dus als volgt uit kunnen zien:

```
myapp/
├──src/
│ ├── app.ts
│ ├── controllers/
│ │   └── mainController.ts
│ ├── db/
│ │   ├── migrations/
│ │   ├── seeds/
│ │   └── knexfile.js
│ ├── entities/
│ │   └── User.entity.ts
│ ├── middleware/
│ │   └── authMiddleware.ts
│ ├── public/
│ │   └── styles.css
│ ├── routes/
│ │   └── mainRoutes.ts
│ └── views/
│     └── home.ejs
├── ormconfig.json
├── package.json
├── tsconfig.json
```

Er is dus een folder bijgekomen genaamd **db** met daarin folders voor migration-bestanden en een folder voor seeders, daarnaast het config bestand.
Nu is Knex.js geïnstalleerd, geconfigureerd en is de folderstructuur opgezet. Je kunt nu aan de slag met het uitvoeren van migraties en het creëren van seeds.




