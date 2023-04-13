# Queries in TypeORM

## Methodes

TypeORM biedt verschillende manieren om queries te schrijven en uit te voeren, die vergelijkbaar zijn met de methoden die in Laravel worden gebruikt. Hieronder vindt je uitleg over enkele van deze methoden, samen met codevoorbeelden die specifiek zijn gericht op een ervaren Laravel-developer.

**EntityManager**: Dit is een object dat verantwoordelijk is voor het beheren van entities en het uitvoeren van database-operaties. Het EntityManager-object kan worden gebruikt om basis-crud-operaties uit te voeren, zoals het opslaan, opvragen, bijwerken en verwijderen van entities. Hier is een voorbeeld van het opslaan van een entity met behulp van EntityManager:

```javascript
const user = new User();
user.firstName = 'John';
user.lastName = 'Doe';

const entityManager = getManager();
await entityManager.save(user);
```

**QueryBuilder**: Dit is een object dat gebruikt kan worden om meer geavanceerde queries te schrijven en uit te voeren. Het **QueryBuilder-object** wordt meestal gebruikt om geavanceerde select-queries uit te voeren die gebruik maken van joins, filters, groeperingen, enz. Hier is een voorbeeld van het uitvoeren van een select-query met behulp van QueryBuilder:

```javascript
const queryBuilder = getRepository(User)
  .createQueryBuilder('user')
  .leftJoinAndSelect('user.posts', 'posts')
  .where('user.firstName = :firstName', { firstName: 'John' });

const users = await queryBuilder.getMany();
```

> Het EntityManager-object is vergelijkbaar met het Model-object in Laravel, terwijl het QueryBuilder-object vergelijkbaar is met de Eloquent query builder.

## Subqueries

Subqueries zijn sub-select-statements die binnen een hoofdquery worden uitgevoerd. In TypeORM kun je subqueries schrijven door gebruik te maken van de **QueryBuilder-object**. Je kunt subqueries schrijven om complexere select-statements te creëren of om gegevens uit meerdere tabellen op te halen. Hier is een voorbeeld van hoe een subquery in TypeORM zou kunnen worden geschreven:

```javascript
const subQuery = await connection
  .createQueryBuilder()
  .select("MAX(age)", "maxAge")
  .from(User, "user")
  .groupBy("user.companyId")
  .subQuery();

const users = await connection
  .createQueryBuilder()
  .select("user")
  .from(User, "user")
  .innerJoin(subQuery, "sq", "user.age = sq.maxAge")
  .getMany();
```

**Joins** zijn queries die gegevens uit twee of meer tabellen samenvoegen. In TypeORM kun je joins schrijven door gebruik te maken van de **innerJoin- of leftJoin-methoden** van de `QueryBuilder`. Hier is een voorbeeld van hoe een **inner join** in TypeORM zou kunnen worden geschreven:

```javascript
const posts = await connection
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
import { EntityRepository, Repository } from 'typeorm';
import { User } from './user.entity';

@EntityRepository(User)
export class UserRepository extends Repository<User> {
  findByEmail(email: string): Promise<User> {
    return this.findOne({ where: { email } });
  }
}
```

In dit voorbeeld creëren we een **repository voor de User-entiteit**. We extenden de `Repository-class` en geven de User-entiteit mee als argument. Hierdoor krijgen we toegang tot de basis-methoden van de Repository-klasse en kunnen we specifieke methoden voor onze User-entiteit implementeren, zoals de `findByEmail` methode in dit voorbeeld.