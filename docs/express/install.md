# Installatie en folderstructuur

## Installatie

Hier is een voorbeeld van hoe je een nieuw Express.js-project kunt opzetten met behulp van de **npm-packagemanager**.
De eerste stappen worden gedaan in je terminal:

Maak een nieuwe map voor jouw project(Windows):

```bash
mkdir myapp
cd myapp
```

Initialiseer een nieuw npm-project:

```bash
npm init -y
```

Installeer de benodigde dependencies:

```bash
npm install express dotenv
```

> De dotenv package word gebruikt om de environment variables in het .env bestand uit te kunnen lezen.

Vervolgens installeren we de volgende dev-dependencies om **Typescript** op te zetten(De **-D** ookwel **--dev** flag is een specifiatie voor de npm packagemanager):

```bash
npm i -D typescript @types/express @types/node
```

Nu genereren we een config bestand voor Typescript, voor nu is de default prima en de config opties behandelen we op een later punt:

```bash
npx tsc --init
```

Als laatste dependency voegen we **ts-node-dev** toe, deze package herstart de applicatie wanneer er veranderingen in de folder worden geconstateerd(**Hot-module reloading**):

```bash
npm install -D ts-node-dev
```

Voeg nu in `package.json` onder de key **scripts** het volgende command toe:

```json
"scripts": {
    "dev": "ts-node-dev --respawn --transpile-only --exit-child --watch src src/app.ts",
    // Other scripts...
  }
```

De config opzet is voor nu gereed, we maken nu eerst een folder genaamd **src** en hierin een bestand voor de hoofdapplicatie, bijvoorbeeld **app.ts**:

Open **app.ts** in je IDE en voeg de volgende code toe:

```javascript
import express, { Express, Request, Response } from "express";
import dotenv from "dotenv";

dotenv.config();

const app: Express = express();

app.get("/", (req: Request, res: Response) => {
  res.send("Hello World!");
});

const port = 3000;
app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});
```

Start de applicatie door het volgende commando uit te voeren in je terminal:

```bash
npm run dev
```

Ga naar http://localhost:3000 in een webbrowser en je zult **"Hello World!"** zien.
Met deze stappen heeft je een eenvoudige Express.js-applicatie opgezet. Je kunt deze nu uitbreiden door een folderstructuur op te zetten en hieraan je routes, controllers en middleware toe te voegen.

## Folderstructuur

Express.js is een unopinionated framework en geeft de developer alle vrijheid zijn of haar eigen structuur te bepalen zo ook de folderstructuur. Een simpele folderstructuur voor een Express.js-applicatie zou er als volgt uit kunnen zien:

```
myapp/
├──src/
│ ├── app.ts
│ ├── controllers/
│ │   └── mainController.ts
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

**src/**: De hoofdmap voor de bronbestanden van de applicatie.

**app.ts**: Dit is het hoofdbestand van uw Express.js-applicatie, waarin je de instellingen configureert, middleware toevoegt en routes definieert.

**controllers/**: Dit is de map waarin je jouw controllers opslaat. Controllers bevatten de logica die nodig is om requests te verwerken en responses terug te sturen.

**middleware/**: Dit is de map waarin je jouw middleware opslaat. Middleware is een soort code die tussen de requests en responses van de gebruiker en de server wordt uitgevoerd.

**models/**: Dit is de map waarin je jouw datamodellen opslaat. Modellen zijn objecten die jouw applicatie gebruikt om gegevens op te slaan, bij te werken en te verwijderen.

**public/**: Dit is de map waarin je jouw statische bestanden opslaat, zoals CSS- en JavaScript-bestanden.

**routes/**: Dit is de map waarin je jouw routes opslaat. Routes zijn de paden waarlangs gebruikers jouw applicatie kunnen benaderen en de bijbehorende functies die worden uitgevoerd als een gebruiker een specifiek pad bezoekt.

**views/**: Dit is de map waarin je jouw view-templates opslaat. De templates zijn bestanden die jouw applicatie gebruikt om dynamische HTML-pagina's te renderen.

**package.json**: Bevat informatie over de npm-pakketten die nodig zijn voor de applicatie.

**tsconfig.json**: Bevat de TypeScript-configuratie-instellingen voor de applicatie.

Dit is slechts een voorbeeld van een folderstructuur en je kunt het aanpassen aan de specifieke vereisten en projectgrootte.

#### Lees meer

- [Express.js setup guides](http://expressjs.com/en/starter/installing.html)
