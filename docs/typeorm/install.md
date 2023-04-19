# Installeren van TypeORM

Je kunt TypeORM gemakkelijk installeren vanuit je terminal met npm of yarn:

```bash
npm install typeorm reflect-metadata mysql(of mysql2)

of

yarn add typeorm reflect-metadata mysql(of mysql2)
```

## Configureren van TypeORM

Nadat TypeORM is geïnstalleerd, maken we een nieuwe folder **database** aan in de **src-folder**. en kun je TypeORM configureren door een nieuw bestand `ormconfig.json` of `ormconfig.ts` te gebruiken. Hieronder vindt je een voorbeeld van de inhoud van een ormconfig.ts-bestand:

```javascript
import 'reflect-metadata';
const parentDir = join(__dirname, "..");

const connectionOpts: MysqlConnectionOptions = {
  type: "mysql",
  host: process.env.DB_HOST,
  port: parseInt(process.env.DB_PORT!),
  username: process.env.DB_USERNAME,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE,
  entities: [`${parentDir}/**/*.entity.{ts,js}`],
  synchronize: false,
  migrationsRun: false, // Migrations are run through Knexjs
  extra: {
    connectionLimit: parseInt(process.env.DB_CONNECTION_LIMIT!),
  },
  logger: undefined,
};

export const AppDataSource = new DataSource(connectionOpts);

export const dbConnection = async () => {
  await AppDataSource.initialize();
};

```

In dit voorbeeld gebruiken we een **MySQL-database**, maar je kunt ook andere database typen gebruiken, zoals PostgreSQL, MSSQL, SQLite en Oracle en is is ondersteuning voor no sql databases als MongoDb.

In **app.ts** roepen we nu de functie aan om de database connectie te leggen:

```javascript
app.listen(port, async () => {
  // Start database connection
  await dbConnection();

  console.log(`App listening on port ${port}`);
});
```

In onze **tsconfig.json** voegen we de volgende opties toe:

```json
"compilerOptions": {
  "emitDecoratorMetadata": true,
  "experimentalDecorators": true,
  // Other options..
}

```

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
│ ├── database/
│ │   └── ormconfig.ts
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
├── package.json
├── tsconfig.json
```

Er is dus nu in de **rootfolder** een config bestand bijgekomen en vanaf nu definiëren we de **Entities** die TypeORM gebruikt in de desbetreffende **entitiesfolder**.

#### Lees meer

- [Typeorm opzetten](https://typeorm.io/example-with-express)
