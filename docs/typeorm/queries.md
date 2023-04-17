# Queries in TypeORM

## Methodes

TypeORM biedt verschillende manieren om queries te schrijven en uit te voeren, die vergelijkbaar zijn met de methoden die in Laravel worden gebruikt. Hieronder vindt je uitleg over enkele van deze methoden, samen met codevoorbeelden die specifiek zijn gericht op een ervaren Laravel-developer.

## Active Record vs Data Mapper

Er zijn twee populaire ORM patterns voor queries naar de database: `Active Record` en `Data Mapper`. TypeORM ondersteunt beide patterns, waardoor je kunt kiezen welk patroon het beste bij jouw applicatie past.

Het belangrijkste verschil tussen deze patronen is de manier waarop entiteiten omgaan met hun persistente gegevens. In het **Active Record patroon** heeft elke entiteit zijn eigen methoden om te communiceren met de database(Hiervoor moet de Entity BaseEntity extenden!), terwijl in een **Data Mapper patroon** de entiteiten losstaan van de database en heeft de mapper deze taak.

Als je applicatie eenvoudig is en je snel en gemakkelijk gegevens wilt ophalen, bewerken en opslaan, dan is Active Record een goede keuze. Als je daarentegen een grotere en complexere applicatie hebt waarbij het belangrijk is om de database en de entiteiten gescheiden te houden, is Data Mapper de betere keuze. Lees meer over deze patterns in de [docs].

TypeORM ondersteunt beide patronen en geeft je de vrijheid om te kiezen welk patroon het beste past bij jouw applicatie.

[docs]: https://typeorm.io/active-record-data-mapper

**Active Records**: In Active Record implementeert elk model de basis CRUD-operaties (Create, Read, Update, Delete), en het maakt gebruik van de metadata van het model om automatisch query's op te stellen en op te halen.
Hier zijn een aantal voorbeelden:

```javascript
// create a new user
const user = new User();
user.name = "John Doe";
user.email = "john.doe@example.com";

// save the user to the database
await user.save();

// find all users
const users = await User.find();

// delete a user
await user.remove();
```

**EntityManager**: Dit is een object dat verantwoordelijk is voor het beheren van entities en het uitvoeren van database-operaties. Het EntityManager-object kan worden gebruikt om basis-crud-operaties uit te voeren, zoals het opslaan, opvragen, bijwerken en verwijderen van entities. Hier is een voorbeeld van het opslaan van een entity met behulp van EntityManager:

```javascript
const user = new User();
user.firstName = "John";
user.lastName = "Doe";

const entityManager = getManager();
await entityManager.save(user);
```

**QueryBuilder**: Dit is een object dat gebruikt kan worden om meer geavanceerde queries te schrijven en uit te voeren. Het **QueryBuilder-object** wordt meestal gebruikt om geavanceerde select-queries uit te voeren die gebruik maken van joins, filters, groeperingen, enz. Querybuilders zijn krachtig om dat je de entity as is terug kan geven(getOne,getMany) of een nieuw object kan creëren(Let op: gebruik getRawOne, getRawMany). Hier is een voorbeeld van het uitvoeren van een select-query met behulp van QueryBuilder:

```javascript
const queryBuilder = getRepository(User)
  .createQueryBuilder("user")
  .leftJoinAndSelect("user.posts", "posts")
  .where("user.firstName = :firstName", { firstName: "John" });

const users = await queryBuilder.getMany();
```

> Goed om te weten is dat Entity-manager en getRepository onderdeel zijn van het Data mapper patroon.

Het belangrijkste verschil tussen de entitymanager en repositories is dat de entitymanager verantwoordelijk is voor het beheer van de volledige databasecontext, terwijl repositories zich specifiek richten op het beheer van een bepaalde entiteit. De entitymanager kan worden gebruikt om transacties te beheren en om meerdere repositories tegelijkertijd te gebruiken, terwijl repositories zich richten op de functionaliteit die specifiek is voor de entiteit die ze vertegenwoordigen.

> Het EntityManager-object is vergelijkbaar met het Model-object in Laravel, terwijl het QueryBuilder-object vergelijkbaar is met de Eloquent query builder.

## Subqueries

Subqueries zijn sub-select-statements die binnen een hoofdquery worden uitgevoerd. In TypeORM kun je subqueries schrijven door gebruik te maken van de **QueryBuilder-object**. Je kunt subqueries schrijven om complexere select-statements te creëren of om gegevens uit meerdere tabellen op te halen. Hier is een voorbeeld van hoe een subquery in TypeORM zou kunnen worden geschreven:

```javascript
const subQuery = getRepository(User)
  .createQueryBuilder()
  .select("MAX(age)", "maxAge")
  .from(User, "user")
  .groupBy("user.companyId")
  .subQuery();

const users = getRepository(User)
  .createQueryBuilder()
  .select("user")
  .from(User, "user")
  .innerJoin(subQuery, "sq", "user.age = sq.maxAge")
  .getMany();
```

**Joins** zijn queries die gegevens uit twee of meer tabellen samenvoegen. In TypeORM kun je joins schrijven door gebruik te maken van de **innerJoin- of leftJoin-methoden** van de `QueryBuilder`. Hier is een voorbeeld van hoe een **inner join** in TypeORM zou kunnen worden geschreven:

```javascript
const posts = getRepository(User)
  .createQueryBuilder()
  .select("post")
  .from(Post, "post")
  .innerJoin(User, "user", "post.authorId = user.id")
  .getMany();
```

## Repository

Een TypeORM repository is een **abstractie-laag** die de interactie met de database vergemakkelijkt. Het biedt een aantal basis-methoden die gebruikt kunnen worden om met de database te communiceren, zoals het opslaan, bijwerken, verwijderen en ophalen van entiteiten.

Een voorbeeld van het gebruik van een repository in TypeORM is als volgt:

```javascript
import { EntityRepository, Repository } from "typeorm";
import { User } from "./user.entity";

@EntityRepository(User)
export class UserRepository extends Repository<User> {
  findByEmail(email: string): Promise<User> {
    return this.findOne({ where: { email } });
  }
}
```

```javascript
import { getCustomRepository } from "typeorm";
import { UserRepository } from "./path/to/user.repository";

// ...

const userRepository = getCustomRepository(UserRepository);
const user = await userRepository.findByEmail("voorbeeld@email.com");
```

In dit voorbeeld creëren we een **repository voor de User-entiteit**. We extenden de `Repository-class` en geven de User-entiteit mee als argument. Hierdoor krijgen we toegang tot de basis-methoden van de Repository-klasse en kunnen we specifieke methoden voor onze User-entiteit implementeren, zoals de `findByEmail` methode in dit voorbeeld.
