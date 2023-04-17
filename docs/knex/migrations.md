# Migrations

## Basis opzet migratie-bestand

Migraties in Knex.js zijn een belangrijk onderdeel van het beheer van de database-structuur in je project. Hieronder is het proces van het aanmaken, bijwerken en verwijderen van tabellen, velden en indexen met behulp van migraties in Knex.js.

**Aanmaken van een migratie:** Voer de volgende opdracht uit in jouw terminal om een nieuwe migratie aan te maken: `npm run knex:migrate:make [naam van migratie]`. Dit zal een nieuw bestand creëren in de migrations-folder waarin je jouw migratie kunt schrijven.

**Aanmaken van tabellen:** In de migratiebestanden, kun jouw tabellen aanmaken met behulp van de `.createTable` methode van Knex.js. Deze bestanden bevatten altijd een up en down functie.

```javascript
exports.up = function (knex) {
  return knex.schema.createTable("users", (table) => {
    table.increments("id").primary();
    table.string("name").notNullable();
    table.timestamp("created_at").defaultTo(knex.fn.now());
    table.timestamp("updated_at").defaultTo(knex.fn.now());
  });
};

exports.down = function (knex) {
  return knex.schema.dropTable("users");
};
```

**Bijwerken van tabellen:** Je kunt tabellen bijwerken door de .table-methode te gebruiken om een bestaande tabel op te halen en vervolgens de tabel aanpassen, aanvullen enzovoort eventueel kun je de `addColumn`, `.dropColumn`, en `.renameColumn` methoden te gebruiken om velden toe te voegen, te verwijderen en te hernoemen. Hieronder is een voorbeeld van het toevoegen van een nieuwe column **'bio'** aan de 'users'-tabel:

```javascript
exports.up = function (knex) {
  return knex.schema.table("users", (table) => {
    table.text("bio");
    table.string("email").alter();
  });
};

exports.down = function (knex) {
  return knex.schema.table("users", (table) => {
    table.dropColumn("bio");
    table.string("email").notNullable().alter();
  });
};
```

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
   public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->text('bio')->after('email');
        });
    }

    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->dropColumn('bio');
        });
    }
```

</details>
<hr />

**Verwijderen van tabellen:** Je kunt tabellen verwijderen door de `.dropTable` methode te gebruiken. Hieronder is een voorbeeld van het verwijderen van de 'users'-tabel:

```javascript
exports.down = function (knex) {
  return knex.schema.dropTable("users");
};
```

## Rollbacks

Met Knex.js kun je een specifieke migratie terugrollen door de `knex.migrate:rollback` opdracht uit te voeren in de terminal. Deze opdracht zal de laatst uitgevoerde migratie ongedaan maken, dus als je meerdere migraties hebt uitgevoerd, zul je mogelijk meerdere keren deze opdracht moeten uitvoeren om terug te gaan naar de gewenste staat.

Bijvoorbeeld, als je naar de laatste migratie wilt terugkeren, kun je de volgende opdracht uitvoeren:

```bash
npx knex migrate:rollback
```

Als je wilt specificeren hoeveel migraties je wilt terugrollen, kun je de `--all` optie gebruiken:

```bash
npx knex migrate:rollback --all
```

> Let op, dit zijn de directe commands voor het terugrollen van migrations, hiervoor moet je terminal zich in de database folder bevinden, vanaf root kun je onze aangemaakte scripts uit de vorige sectie kunnen gebruiken.

Dit zal alle migraties ongedaan maken die zijn uitgevoerd sinds het begin van de migratie-geschiedenis.

Houd er rekening mee dat als je een migratie terugrolt, de aanpassingen die zijn gemaakt aan de database zullen worden verwijderd. Het is belangrijk om een backup te maken van de database voordat je migraties terugrolt, zodat je deze informatie niet permanent verliest.

## Batch-migraties

Met Knex.js kun je meerdere migraties in één batch uitvoeren door de `knex.migrate:latest` opdracht uit te voeren in de terminal. Deze opdracht zal alle migraties uitvoeren die nog niet zijn uitgevoerd, zodat je altijd up-to-date bent met de meest recente migraties.

Bijvoorbeeld, om alle migraties uit te voeren die nog niet zijn uitgevoerd, kun je de volgende opdracht uitvoeren:

```bash
npx knex migrate:latest
```

> Let op, dit is het directe command voor het runnen van migrations, hiervoor moet je terminal zich in de database folder bevinden, vanaf root kun je onze aangemaakte scripts uit de vorige sectie kunnen gebruiken.

Met de **migratie-geschiedenis** kun je beter beheren door ervoor te zorgen dat elke migratie uniek is. Dit kun je bereiken door elke migratie een unieke naam te geven en door een nauwkeurige beschrijving van de migratie op te nemen in het migratiebestand.

Het is ook aan te bevelen om regelmatig migraties uit te voeren en de migratie-geschiedenis bij te houden, zodat je altijd weet welke migraties zijn uitgevoerd en wanneer. Hierdoor kun je eventuele problemen op een eenvoudiger manier opsporen en oplossen.

Omdat migraties aanpassingen aanbrengen aan de database, is het belangrijk om altijd een backup te maken van de database voordat je migraties uitvoert. Dit zorgt ervoor dat je deze informatie niet permanent verliest als er iets fout gaat tijdens het uitvoeren van de migraties.

## Volledig voorbeeld

Hieronder is een voorbeeld van het aanmaken van een **'users'-tabel**:

```javascript
import knex, { Knex } from "knex";

export async function up(knex: Knex): Promise<void> {
  return knex.schema.createTable("users", (table) => {
    table.increments("id");
    table.string("name", 500).notNullable();
    table.string("email", 500).notNullable();
    table.string("email_lookup", 500).notNullable();
    table.string("password", 500).notNullable();
    table.string("remember_token", 500).nullable();
    table.string("permissions", 500).nullable();
    table.timestamp("created_at").defaultTo(knex.fn.now());
    table
      .timestamp("updated_at")
      .defaultTo(knex.raw("CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP"));
  });
}

export async function down(knex: Knex): Promise<void> {
  return knex.schema.dropTableIfExists("users");
}
```
