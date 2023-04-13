# Installeren van TypeORM

Je kunt TypeORM gemakkelijk installeren vanuit je terminal met npm of yarn:

```bash
npm install typeorm

of

yarn add typeorm
```

## Configureren van TypeORM

Nadat TypeORM is geïnstalleerd, kun je het configureren door een nieuw bestand `ormconfig.json` te maken of een bestand `ormconfig.js` te gebruiken. Hieronder vindt je een voorbeeld van de inhoud van een ormconfig.js-bestand:

```javascript

const connectionOpts: MysqlConnectionOptions = {
  type: "mysql",
  host: process.env.DB_HOST,
  port: Number(process.env.DB_PORT),
  username: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE,
  entities: [`${parentDir}/**/*.entity.{ts,js}`],
  synchronize: false,
  migrationsRun: false, // Migrations are run through Knexjs
  extra: {
    connectionLimit: lodash.parseInt(process.env.DB_CONNECTION_LIMIT!),
  },
  logger: undefined
};

export const AppDataSource = new DataSource(connectionOpts);

export const dbConnection = async () => {
  await AppDataSource.initialize();
};


```

In dit voorbeeld gebruiken we een **PostgreSQL-database**, maar je kunt ook andere database typen gebruiken, zoals MySQL, MSSQL, SQLite en Oracle en is is ondersteuning voor no sql databases als MongoDb.

> ##### Vergelijkingen met Laravel
>
> TypeORM doet denken aan de Eloquent ORM die in Laravel wordt gebruikt. Zo gebruikt TypeORM ook entities voor het opzetten van modellen, migrations voor het beheren van de database-schema's en repositories voor het uitvoeren van database-operaties. In Laravel worden deze aspecten ook aangeboden en is het instellen van de database-verbinding vergelijkbaar met het configureren van TypeORM.

## Aanvulling in folderstructuur

De folder structuur voor een Express.js-applicatie met TypeORM zou er nu dus als volgt uit kunnen zien:

```
myapp/
├──src/
│ ├── app.ts
│ ├── controllers/
│ │   └── mainController.ts
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

Er is dus nu in de **rootfolder** een config bestand bijgekomen en vanaf nu definiëren we de **Entities** die TypeORM gebruikt in de desbetreffende **entitiesfolder**.
