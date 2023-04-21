# Installeren KnexJs

Knex.js is een database-toolkit voor JavaScript die het uitvoeren van database-operaties, zoals het creëren van tabellen en het uitvoeren van migraties, vergemakkelijkt. Hieronder staan de stappen voor het installeren, configureren en instellen van de folderstructuur van Knex.js.

Je installeert Knex.js via npm door het volgende command uit te voeren in de terminal:

```bash
npm install knex
```

## Configureren

Voor het configureren word een `knexfile.ts`-bestand aan gemaakt in een nieuwe folder genaamd **database** van in het project. Een voorbeeld van een basisconfiguratie kan er als volgt uit zien:

```javascript
// knexfile.ts

const knexConfig = {
  client: "mysql2",
  connection: {
    host: process.env.DB_HOST,
    port: parseInt(process.env.DB_PORT!),
    username: process.env.DB_USERNAME,
    password: process.env.DB_PASSWORD,
    database: process.env.DB_DATABASE,
  },
  migrations: {
    tableName: "knex_migrations",
    extension: "ts",
    directory: "migrations",
    disableMigrationsListValidation: true,
    loadExtensions: [".ts"],
  },
  seeds: {
    extension: "ts",
    directory: "seeds",
  },
};

module.exports = {
  development: knexConfig,
  production: knexConfig,
};
```

Nu de configuratie is opgezet gaan we een aantal commands aan onze package.json toevoegen om deze structuur te hanteren, deze zullen we later weer gebruiken in de migrations en seeders secties.

Voeg in `package.json` onder de key **scripts** het volgende command toe:

```json
"scripts": {
    "knex:migrate:make": "knex --knexfile src/database/knexfile.ts migrate:make -x ts",
		"knex:migrate:latest": "knex --knexfile src/database/knexfile.ts migrate:latest",
		"knex:migrate:rollback": "knex --knexfile src/database/knexfile.ts migrate:rollback",
		"knex:seed:make": "knex --knexfile src/database/knexfile.ts seed:make -x ts",
		"knex:seed:run": "knex --knexfile src/database/knexfile.ts seed:run",
    // Other scripts...
  }
```

## Aanvulling in folderstructuur

De folder structuur voor je Express.js-applicatie zou er nu dus als volgt uit kunnen zien:

Er zijn dus folders bijgekomen in de **database-folder** voor migration-bestanden en voor seeders, daarnaast het config bestand.
Nu is Knex.js geïnstalleerd, geconfigureerd en is de folderstructuur opgezet. Je kunt nu aan de slag met het uitvoeren van migraties en het creëren van seeds.

```
myapp/
├──src/
│ ├── app.ts
│ ├── controllers/
│ │   └── mainController.ts
│ ├── database/
│ │   ├── migrations/
│ │   ├── seeds/
│ │   └── knexfile.ts
│ ├── middleware/
│ │   └── authMiddleware.ts
│ ├── public/
│ │   └── styles.css
│ ├── routes/
│ │   └── mainRoutes.ts
│ └── views/
│     └── home.ejs
├── package.json
├── tsconfig.json
```

#### Lees meer

- [Knex opzetten](https://knexjs.org/guide/migrations.html#knexfile-js)
