# TypeORM entities

## Entity definitie

Een **Entity** in TypeORM is een objectrepresentatie van een database tabel. Dit betekent dat elke Entity een class representatie is van een database tabel en elke property van deze class een database kolom representeert.
TypeORM entities zijn vergelijkbaar met Laravel's **Eloquent modellen**. Het is de manier waarop je de databasetabellen in je Express.js-applicatie modelleert en opstelt. Hieronder is een voorbeeld van een eenvoudige TypeORM-entity:

```javascript
import { Entity, Column, PrimaryGeneratedColumn, BaseEntity } from 'typeorm';

@Entity()
export class User extends BaseEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  email: string;

  @Column()
  password: string;
}

```

In dit voorbeeld is er een class User gemaakt die gedecoreerd is met `@Entity`. Hierdoor wordt het TypeORM gezegd dat deze class een Entity is en deze dus moet worden opgeslagen in de database. Zo geeft bijvoorbeeld, de `@Column` decorator geeft aan dat deze kolom in de database moet worden opgenomen.

> BaseEntity geeft CRUD operaties die dan beschikbaar zijn voor elke entity in Typeorm. Het biedt ook een standaardimplementatie voor veelgebruikte velden zoals "createdAt" en "updatedAt". Door deze functionaliteiten in de BaseEntity te definiëren, kunnen ze eenvoudig worden geërfd door andere entities zonder opnieuw te hoeven worden geschreven.

Een belangrijk verschil met Laravel's Eloquent is dat TypeORM de tabelnaam automatisch op basis van de naam van de entity class bepaalt. In dit voorbeeld zou de tabelnaam user zijn. Als je een andere tabelnaam wilt specificeren, kun je de `@Entity` decorator als volgt configureren: `@Entity({ name: 'custom_table_name' })`.
Dit is vergelijkbaar met Laravel's Eloquent ORM, waar elke model class een database tabel representeert en elke property een database kolom.

> TypeORM ondersteunt ook het automatisch genereren van de tabellen, wat bijdraagt aan het gemak en de efficiëntie van het beheer van je database.

In vergelijking met Laravel's Eloquent, is TypeORM meer flexibel en veelzijdiger, maar het vereist ook meer opstarttijd en aanpassing van de developer.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $table = 'users';
    protected $fillable = ['name', 'email', 'password'];
}
```

</details>
<hr />

Het enige verschil is dat in Laravel we gebruik maken van `Illuminate\Database\Eloquent\Model` en in TypeORM de decorators gebruiken om de entity te definiëren. Het gebruik van de tabelnaam en de beveiligde velden zijn echter hetzelfde.

## Relations

In TypeORM, kun je relaties tussen entities instellen door gebruik te maken van **decorators** of door de relaties op de entity te definiëren met behulp van de **EntityManager API**.

Decorators zijn snel en makkelijk te gebruiken, hieronder is een voorbeeld van hoe je een **One-To-Many** relatie tussen twee entities kunt definiëren:

```javascript
import { Entity, Column, BaseEntity, PrimaryGeneratedColumn, OneToMany } from 'typeorm';
import { ChildEntity } from './ChildEntity';

@Entity()
export class ParentEntity extends BaseEntity{
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @OneToMany(type => ChildEntity, child => child.parent)
  children: ChildEntity[];
}

@Entity()
export class ChildEntity extends BaseEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @ManyToOne(type => ParentEntity, parent => parent.children)
  parent: ParentEntity;
}
```

In dit voorbeeld wordt de parent entity gedefinieerd met een primary key `id` en een kolom `name`. De One-To-Many relatie tussen de parent en child entities wordt gedefinieerd door het gebruik van de `@OneToMany` decorator op de parent entity en de `@ManyToOne` decorator op de child entity. Hierdoor worden de children en parent eigenschappen op de entities aangemaakt.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
class Parent extends Model
{
    public function children()
    {
        return $this->hasMany(Child::class);
    }
}

class Child extends Model
{
    public function parent()
    {
        return $this->belongsTo(Parent::class);
    }
}
```

</details>
<hr />

In dit voorbeeld wordt de children relatie tussen de parent en child entities gedefinieerd door de children functie te definiëren op de Parent model en de parent relatie door de parent functie te definiëren op de Child model.