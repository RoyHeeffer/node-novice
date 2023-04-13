# Transacties en Batch-operaties

## Transacties

Transacties in TypeORM kunnen worden gebruikt om meerdere database-operaties te verpakken in een enkele logische transactie die samen uitgevoerd of samen ongedaan gemaakt worden. Dit is een belangrijk concept voor het beheren van de integriteit van de gegevens in de database.

Met TypeORM kun je transacties uitvoeren door gebruik te maken van de EntityManager-klasse. Hiermee kun je de functie transaction uitvoeren en daarin alle database-operaties verpakken die onderdeel zijn van de transactie.

Een voorbeeld van het gebruik van transacties in TypeORM zou als volgt kunnen zijn:

```javascript
import { EntityManager, EntityTransaction, getConnection } from "typeorm";

async function createUserAndProfile(userData, profileData) {
  const connection = getConnection();
  const manager = connection.manager;

  let user, profile;
  await manager.transaction(async (transactionalEntityManager) => {
    user = await transactionalEntityManager.save(User, userData);
    profile = await transactionalEntityManager.save(Profile, {
      ...profileData,
      user,
    });
  });

  return { user, profile };
}
```

In deze functie wordt eerst een verbinding met de database gemaakt via getConnection(), en vervolgens een EntityManager-object aangemaakt. Hiermee wordt de functie transaction uitgevoerd, en alle database-operaties worden verpakt in deze transactie.

In vergelijking met Laravel's Eloquent, is de manier waarop transacties worden uitgevoerd en beheerd in TypeORM vergelijkbaar, maar met enkele subtiele verschillen in de syntax en methoden die beschikbaar zijn. Bijvoorbeeld, in Laravel kun je transacties uitvoeren met behulp van de methode beginTransaction op een model of via de methoden DB::transaction en DB::beginTransaction in de Query Builder.

In beide frameworks zijn transacties een belangrijk onderdeel van het beheer van de integriteit van de gegevens in de database, en beide bieden manieren om de transacties te beheren en te waarborgen dat de gegevens consistent blijven.

Een transactie in Laravel kan op de volgende manier worden uitgevoerd:

```php

use Illuminate\Support\Facades\DB;

Route::get('/test', function () {
    DB::beginTransaction();

    try {
        // Voer hier de database-operaties uit
        DB::commit();
    } catch (\Exception $e) {
        DB::rollBack();
        return $e->getMessage();
    }
});
```

In het bovenstaande voorbeeld wordt DB::beginTransaction() gebruikt om een transactie te starten en DB::commit() wordt gebruikt om de transactie te bevestigen. Als er een fout optreedt, wordt de transactie met DB::rollBack() ongedaan gemaakt. Hiermee voorkom je dat ongewenste veranderingen in de database worden doorgevoerd.

In Laravel worden transacties vaak gebruikt in combinatie met Eloquent ORM. Hierdoor kun je Eloquent-modellen gebruiken om gegevens op te slaan, bij te werken of te verwijderen binnen een transactie.

## Batch-operaties

Batch-operaties zijn operaties die op meerdere rijen in de database tegelijkertijd worden uitgevoerd. In TypeORM kun je batch-operaties uitvoeren door gebruik te maken van de `EntityManager`. Hier is een voorbeeld van hoe je een batch-update in TypeORM zou kunnen uitvoeren:

```javascript
await connection.transaction(async (transactionalEntityManager) => {
  const users = await transactionalEntityManager
    .createQueryBuilder()
    .update(User)
    .set({ firstName: "John" })
    .where("id IN (:...ids)", { ids: [1, 2, 3] })
    .execute();
});
```

De gebruikte methoden in TypeORM voor het uitvoeren van queries en operaties op de database zijn vergelijkbaar met deze in Laravel.
