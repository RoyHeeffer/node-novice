# Routing in ExpressJs

### Route opzet

Express.js routing is het proces waarbij een bepaalde URI (Uniform Resource Identifier) wordt gekoppeld aan een functie die de specifieke functionaliteit voor die URI uitvoert. Dit wordt gedaan door gebruik te maken van de methoden `app.get(), app.post(), app.put(), app.delete()` etc..

In Express.js kunt je een route definiëren met deze methoden en de URI en de callback functie die uitgevoerd moet worden als argumenten gebruiken. Hieronder een voorbeeld van hoe een route wordt gedefinieerd in Express.js:

```javascript
import express from "express";

const app = express();

app.get("/", (req: Request, res: Response) => {
  res.send("Hello World!");
});
```

In dit voorbeeld wordt de route `'/'` gedefinieerd met de methode `app.get()`. Elke keer dat een **GET-request** naar deze route wordt verstuurd, zal de functie die als tweede argument is meegegeven worden uitgevoerd en **"Hello World!"** worden geretourneerd.

In PHP met Laravel wordt routing gedaan met behulp van de Router class. Deze class biedt methoden zoals `get(), post(), put() en delete()` voor het definiëren van routes.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
Route::get("/", function () {
  return "Hello World!";
});

```

</details>
<hr />

In dit voorbeeld wordt de route `'/'` gedefinieerd met de methode `Route::get()`. Als een **GET-request** naar deze route wordt verstuurd, zal de functie die als tweede argument is meegegeven worden uitgevoerd en "Hello World!" worden geretourneerd.

De basisconcepten zijn vergelijkbaar, maar de syntax is iets anders. In Express.js maak je gebruik van de `app-context`, terwijl in Laravel gebruik word gemaakt van de `Route-facade`.

## Dynamische routes en route params

In Express.js kunt je ook parameters doorgeven aan de route, bijvoorbeeld een gebruikersnaam. Hieronder een voorbeeld van hoe dit wordt gedaan in Express.js:

```javascript
app.get("/users/:username", (req: Request, res: Response) => {
  const { username } = req.params;
  res.send(`Hello, ${username}`);
});
```

De waarde van deze parameter is beschikbaar via het `req.params` object.
In dit voorbeeld wordt de route `'/users/:username'` gedefinieerd met de methode `app.get()` en de gebruikersnaam wordt opgevraagd uit de `req.params`. Let hierbij goed op de `:` voor username in de route, dit geeft aan dat deze waarde dynamisch is en vindbaar onder de routeparam key `username`.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
Route::get('/users/{username}', function($username) {
    return "Hello, ".$username;
});
```

</details>
<hr />

Je kunt ook gebruik maken van **query parameters**, die zijn beschikbaar via het `req.query` object. Hier is een voorbeeld van hoe je een route kunt maken die een zoekopdracht verwerkt:

```javascript
app.get("/search", (req: Request, res: Response) => {
  res.send(`Search results for: ${req.query.q}`);
});
```

In dit voorbeeld, verwacht de route een query parameter met de naam `q`. Bijvoorbeeld, de volgende URL zou de zoekresultaten voor **"Express js"** weergeven: `/search?q=Express js`

## Modulaire routing

Express.js heeft een **gedecentraliseerd routing systeem**, in tegenstelling tot Laravel's centraal gestuurd systeem. In Express.js worden routes vaak gedefinieerd in een aparte bestand of module, in plaats van in een enkele bestandslocatie zoals bij Laravel. Hier is een voorbeeld van hoe routes worden gedefinieerd in Express:

```javascript
import express from "express";
const router = express.Router();

router.get("/", (req: Request, res: Response) => {
  res.send("Hello World!");
});

router.get("/about", (req: Request, res: Response) => {
  res.send("About page");
});

export default router;
```

In dit voorbeeld, worden twee routes gedefinieerd: een voor de homepage (`'/'`) en een voor de pagina `'about'`. Het verschil tussen deze twee routes is de URL-pad en de callback-functie die wordt uitgevoerd wanneer de route wordt opgeroepen.

In laravel worden de routes gedefinieerd in een enkele bestand **routes/web.php** of **routes/api.php**.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
Route::get('/', function () {
    return view('welcome');
});

Route::get('/about', function () {
    return view('about');
});

```

</details>
<hr />

Als je kijkt naar deze twee voorbeelden zie je dat de manier van routes definieren ongeveer hetzelfde is als in laravel. Het grootste verschil is de locatie waar de routes worden gedefinieerd en de manier waarop ze worden geëxporteerd in Express.js

> In Express.js-routing is het belangrijk om een goede response te geven op de verschillende soorten verzoeken. De meest voorkomende responses zijn `res.send()` en `res.json()` je kunt ook de status-code aanpassen bijvoorbeeld `res.status(404).json({message: "not found"})`, Lees hier meer over [requests] in Express.

## Volledig voorbeeld

Omdat routes worden gedefinieerd in aparte bestanden of modules en moeten deze worden geëxporteerd naar de hoofdapplicatie. Een voorbeeld van een router-module in Express.js zou er als volgt uit kunnen zien:

```javascript
/* /routes/mainRoutes.ts */

import express from "express";
const router = express.Router();

// Define routes
router.get("/", (req: Request, res: Response) => {
  res.send("Hello, world!");
});

router.get("/about", (req: Request, res: Response) => {
  res.send("About page");
});

router.get("/users/:id", (req: Request, res: Response) => {
  res.send(`User ID: ${req.params.id}`);
});

export default router;
```

```javascript
/* index.ts */

import express from "express";
import router from "./routes/router";

const app = express();

// Use the router module
app.use("/", router);

app.listen(3000, () => {
  console.log("Server listening on port 3000");
});
```

In bovenstaand voorbeeld, importeert de hoofdapplicatie de router-module met de naam router uit het bestand **routes/router.js**. De `app.use()` functie wordt gebruikt om de router-module te gebruiken voor verzoeken die beginnen met `'/'`. Dit betekent dat de routes gedefinieerd in de router-module zullen worden uitgevoerd voor verzoeken naar de server met een pad dat begint met `'/'`.

Dit is vergelijkbaar met hoe routing werkt in Laravel, waar routes worden gedefinieerd in aparte bestanden en worden toegevoegd aan de hoofdapplicatie via een service provider.

Lees hier meer over [routing] in Express.

[requests]: https://expressjs.com/en/5x/api.html#res
[routing]: https://expressjs.com/en/5x/api.html#router
