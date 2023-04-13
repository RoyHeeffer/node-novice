# Introductie ExpressJs

## Global middleware

Express.js heeft ook een middleware systeem dat je kunt gebruiken om routes te beveiligen, te valideren of zelfs te transformeren. Dit is vergelijkbaar met Laravel's middleware systeem, waarmee je specifieke functionaliteit kunt toepassen op routes.

Middleware functies zijn functies die worden uitgevoerd tussen het aanroepen van een route en het verzenden van een antwoord. Hier is een voorbeeld van hoe je een middleware functie kunt gebruiken om een logboek bij te houden van alle aanvragen:

```javascript
// Define a piece of middleware
const loggerMiddleware = (req: Request, res: Response, next: NextFunction) => {
  console.log(`${req.method} request received at ${req.url}`);
  next();
};

// Use middleware
app.use(loggerMiddleware);
```

In dit voorbeeld, de middleware functie logt de methode van de aanvraag (bijv. `GET` of `POST`) en de URL waarop de aanvraag is ontvangen. De `next()` functie wordt aangeroepen om aan te geven dat de volgende stap in de request-response cyclus moet worden uitgevoerd.
De middleware wordt toegevoegd aan de hoofdapplicatie met de `app.use()` functie. Hierdoor zal elke request eerst door deze middleware worden verwerkt voordat het wordt doorgegeven aan een controller of een volgende middleware in de keten.

In Laravel is het vergelijkbaar met het toevoegen van middleware aan een specifieke route via de middleware method of aan een groep van routes met de middlewareGroups method in de Kernel-klasse.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php

class LoggerMiddleware
{
    public function handle($request, Closure $next)
    {
        Log::info("{$request->method()} request received at {$request->url()}");

        return $next($request);
    }
}

```

</details>
<hr />

## Specifieke routes targeten

Je kunt ook gebruik maken van route targeted middleware functies in express.js, deze middleware kun je toevoegen aan een route door het als een derde argument mee te geven aan de route-definitie(overloading), zoals hieronder:

```javascript
app.get("/users/:username", authMiddleware, (req: Request, res: Response) => {
  const { username } = req.params;
  res.send(`Hello, ${username}`);
});
```

Dit zijn slechts enkele van de vele mogelijkheden die Express.js biedt voor het maken van web-applicaties. Je kunt deze basis gebruiken en het uitbreiden met meer routes, functies en middleware om aan jouw specifieke vereisten te voldoen.
