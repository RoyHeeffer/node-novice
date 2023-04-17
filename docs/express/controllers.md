# Controllers in ExpressJs

## Controller class

Controllers in Express.js behandelen de logica van een applicatie en fungeren als een brug tussen de route en de rest van de applicatie. Controllers zijn verantwoordelijk voor het verwerken van **Requests** en het terugsturen van **Responses**. Je kunt een controller maken met behulp van **classes** en deze aan de routes toevoegen. Laravel-controllers daarentegen worden gemaakt met behulp van een specifieke klasse en zijn gekoppeld aan routes met behulp van de `Route::controller()` methode.

> Javascript is van origine geen OOP taal, een class in JS is een syntactische uitbreiding op prototypes en functies die de mogelijkheid biedt om object-georiënteerde programmering te implementeren. in Javascript werken Access-modifiers als Protected, Private en Public dus eigenlijk niet. Typescript leest deze modifiers wel en geeft daarom errors, maar in theorie zou je alles alszijnde static kunnen gebruiken

Net als bij routing, heeft Express.js een **gedecentraliseerde** aanpak voor controllers in tegenstelling tot Laravel's centraal gestuurde aanpak. In Express, worden controllers gedefinieerd in aparte bestanden of modules.

Een voorbeeld van een Express.js controller zou er als volgt uitzien:

```javascript
class UserController {
  public async create(req: Request, res: Response) {
    const user = await User.create({
      name: req.body.name,
      email: req.body.email,
    });
    res.send(user);
  }

  public async update(req: Request, res: Response) {
    const user = await User.findOne({
      where: { id: req.params.id },
    });
    user.name = req.body.name;
    user.email = req.body.email;
    await user.save();
    res.send(user);
  }
}
```

In bovenstaande voorbeeld hebben we een UserController met twee methoden: create en update. De create-methode maakt een nieuwe gebruiker aan op basis van de gegevens die zijn verzonden in de aanvraag. De update-methode zoekt een gebruiker op op basis van de id die is meegegeven in de route en past de gegevens aan.

<hr />
<details>
  <summary>Vergelijkbaar codevoorbeeld in Laravel</summary>

```php
class UserController extends Controller
{
  public function create(Request $request)
  {
    $user = User::create([
    'name' => $request->name,
    'email' => $request->email,
    ]);
    return $user;
  }

  public function update(Request $request, $id)
  {
      $user = User::find($id);
      $user->name = $request->name;
      $user->email = $request->email;
      $user->save();
      return $user;
  }

}
```

</details>
<hr />

Zoals je ziet, zijn de concepten vergelijkbaar, maar de syntax is net iets anders. In Express.js gebruiken we het `Request` en `Response` object om toegang te krijgen tot de aanvraaggegevens en om een reactie te sturen, terwijl we in Laravel gebruik maken van de **Request en de Response facade**.

In de volgende stap van de cursus zou je kunnen laten zien hoe de controller wordt gebruikt in combinatie met routes, zodat de studenten kunnen zien hoe een eindpunt wordt geconfigureerd en afgehandeld in Express.js.

## Volledig voorbeeld

Het grootste verschil is de locatie waar de controllers worden gedefinieerd en de manier waarop ze worden geëxporteerd in Express.js. In Laravel worden controllers gedefinieerd als klassen die zich bevinden in de `app/Http/Controllers` map. Hier is een voorbeeld van het gebruik van een eenvoudige controller in Express:

```javascript
/* /controllers/UserController.ts */

export default class UserController {
  static async getAllUsers(req: Request, res: Response) {
    // Code om gebruikers op te halen uit de database
    const users = await User.find();
    res.json(users);
  }
  static async createUser(req: Request, res: Response) {
    // Code om een gebruiker aan te maken en op te slaan in de database
    const newUser = new User(req.body);
    await newUser.save();
    res.json(newUser);
  }
}
```

```javascript
/* /router/userRoutes.ts */

import express from "express";
import UserController from "../controllers/UserController";
const router = express.Router();

router.get("/users", UserController.getAllUsers);
router.post("/users", UserController.createUser);

export default router;
```

In dit voorbeeld, is er een UserController met twee methodes: `getAllUsers` en `createUser`. Deze methodes worden gebruikt als de callback-functies voor de get en post routes voor de `'/users'` pad.

Bekijk voorbeelden voor meer informatie over [controllers].

[controllers]: hhttps://expressjs.com/en/starter/faq.html#how-should-i-structure-my-application
