# Introductie ExpressJs

## Kort overzicht

Express.js is een open source minimalistisch server-side JavaScript framework dat is gericht op het snel en eenvoudig opzetten van web-applicaties. Het framework is gebaseerd op Node.js en is vrij snel en flexibel in vergelijking met andere back-end frameworks.

**Architectuur**<br/>
De architectuur van Express.js is zo ontworpen dat het makkelijk te begrijpen en uit te breiden is. Het bevat enkele basisfuncties zoals routing, controllers en middleware die zijn opgebouwd uit losse modules en gemakkelijk kunnen worden aangepast of vervangen. Dit maakt Express.js uiterst geschikt voor kleine tot middelgrote projecten, maar ook voor grotere projecten waar meer flexibiliteit vereist is.

**Routing**<br/>
Express.js routing is het systeem waarmee verschillende URL's worden gekoppeld aan specifieke acties. Dit betekent dat je de gebruikers kunt doorsturen naar de juiste pagina op basis van de URL die ze hebben ingevoerd. In Express.js is dit systeem gemakkelijk te installeren en te configureren, en je kunt hierbij gebruik maken van standaard functies of zelf geschreven code.

**Controllers**<br/>
In Express.js worden controllers gebruikt om de logica van de web-applicatie te beheren. Hiermee kun je specifieke acties uitvoeren, zoals het opvragen van gegevens uit een database of het genereren van een antwoord op een gebruikersactie. Dit systeem werkt samen met de routing om ervoor te zorgen dat de gebruikers altijd de juiste informatie ontvangen, ongeacht wat ze doen op jouw website.

**Middleware**<br/>
In Express.js wordt middleware gebruikt om gegevens te verwerken die tussen jouw server en de gebruikers worden verstuurd. Hiermee kunt je bijvoorbeeld beveiligingsmaatregelen implementeren, zoals het verifiëren van een gebruikersinlog of het verifiëren van de geldigheid van de gegevens die worden verzonden. Dit systeem is gemakkelijk te installeren en te configureren, en je kunt hierbij gebruik maken van standaard functies of zelf geschreven code.

In vergelijking met het Laravel framework, is Laravel meer gericht is op gemak en functionaliteit om zo complete webapplicatie te kunnen opzetten, met ingebouwde functionaliteiten zoals authenticatie en autorisatie. Express.js is meer gericht op flexibiliteit en snelheid, en geeft de developer de vrijheid om de architectuur en opbouw van de applicatie volledig zelf te bepalen.

## Core functies

De core functies van Express.js die je het meest gaat tegenkomen zijn `app.set(), app.use() en app.listen()`. Hieronder staan codevoorbeelden van deze functies:

`app.set()`: Dit is de functie die je kunt gebruiken om verschillende instellingen in jouw Express-applicatie te configureren, zoals het instellen van de **view engine** en de view folder. Hier is een voorbeeld:

```javascript
import express from "express";

const app = express();

// Set the view engine to ejs
app.set("view engine", "ejs");

// Set the views folder
app.set("views", path.join(__dirname, "views"));
```

`app.use()`: Deze functie wordt gebruikt om middleware toe te voegen aan jouw Express-applicatie. Hier is een voorbeeld van het toevoegen van de **body-parser middleware** aan jouw applicatie:

```javascript
import express from "express";

const app = express();

// Use the body-parser middleware
app.use(express.json()); // to support JSON-encoded bodies
app.use(express.urlencoded({ extended: true })); // to support URL-encoded bodies
```

`app.listen()`: Dit is de functie die je kunt gebruiken om jouw Express-applicatie te starten en naar requests te luisteren op een bepaalde **poort**. In dit voorbeeld creëren we een server die luistert op poort 3000:

```javascript
import express from "express";

const app = express();

const port = 3000;

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
```

Deze functies vormen de basis van elke Express.js-applicatie. Het is belangrijk om te begrijpen hoe deze functies werken en hoe ze samenwerken om jouw Express-applicatie aan te sturen.

Bekijk ook de [docs] voor meer informatie over deze functies.

[docs]: https://expressjs.com/en
