# Installeren KnexJs

Knex.js is een database-toolkit voor JavaScript die het uitvoeren van database-operaties, zoals het creëren van tabellen en het uitvoeren van migraties, vergemakkelijkt. Hieronder staan de stappen voor het installeren, configureren en instellen van de folderstructuur van Knex.js.

Installeer Knex.js via npm door het volgende commando uit te voeren in de terminal:

```bash
npm install knex
```

## Configureren

Maak een knexfile.js-bestand aan en zet deze in een nieuwe folder genaamd **db** in de **src-folder** van je project en zet de volgende code uit om een basisconfiguratie in te stellen:

```javascript
// knexfile.js

import dotenv from "dotenv";
import lodash from "lodash";

dotenv.config({ path: "../../.env" });

const knexConfig = {
  client: "mysql2",
  connection: {
    database: "my_db",
    user: "username",
    password: "password",
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

Nu de configuratie is opgezet gaan we een aantal commands aan onze package.json toevoegen, deze zullen we later weer gebruiken in de migrations en seeders secties.

Voeg nu in `package.json` onder de key **scripts** het volgende command toe:

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
