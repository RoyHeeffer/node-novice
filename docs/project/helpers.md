# Helpers

Deze simpele helpers zijn bedoeld om je snel op weg te helpen, bekijk de instructies goed en gebruik de documentatie om een beter begrip van deze code te krijgen en waar nodig te verbeteren:

## ExpressJs - helpers

**generateToken**: een helperfunctie die een JSON Web Token (JWT) genereert voor een gebruiker. Deze functie kan bijvoorbeeld worden aangeroepen wanneer een nieuwe gebruiker wordt aangemaakt.

```javascript
import jwt from "jsonwebtoken";
const secret = "my_secret_key";

const generateToken = (user: User) => {
  const payload = {
    id: user.id,
    email: user.email,
    username: user.username,
  };
  const token = jwt.sign(payload, secret, { expiresIn: "1h" });
  return token;
};
```

**requireAuth**: een middleware functie die controleert of de huidige gebruiker geauthenticeerd is. Deze functie kan bijvoorbeeld worden gebruikt om te controleren of een gebruiker toegang heeft tot bepaalde routes.

```javascript
const requireAuth = (req: Request, res: Response, next: NextFunction) => {
  const authHeader = req.headers.authorization;
  if (!authHeader) {
    return res.status(401).json({ error: "Authentication required" });
  }
  const token = authHeader.split(" ")[1];
  try {
    const payload = jwt.verify(token, secret);
    req.user = payload;
    next();
  } catch (error) {
    return res.status(401).json({ error: "Invalid token" });
  }
};
```

**checkPostOwnership**: een middleware functie die controleert of de huidige gebruiker eigenaar van de desbtreffende post is. Deze functie kan bijvoorbeeld worden gebruikt om te controleren of een gebruiker toegang heeft tot bepaalde beheerdersfuncties.

```javascript
const getUserIdFromToken = (token) => {
  const decodedToken = jwt.verify(token, "my_secret_key");
  return decodedToken.userId;
};

const checkPostOwnership = async (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const postId = req.params.id;
  const token = req.headers.authorization.split(" ")[1];
  const userId = getUserIdFromToken(token);
  try {
    const post = await Post.findById(userId);

    if (!post) {
      return res.status(404).json({ message: "Post not found" });
    }

    if (post.user.toString() !== userId) {
      return res.status(401).json({ message: "Unauthorized" });
    }

    next();
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: "Server error" });
  }
};
```

In dit voorbeeld gaan we ervan uit dat de JWT token wordt verzonden via de `Authorization` header, in het formaat **"Bearer token"**.

## KnexJs - helpers

We gaan een docker-container gebruiken om snel een database op te zetten, dit is geen onderdeel van het curriculum.
We gaan hier dus **niet** dieper op in, zorg ervoor dat je de instructies goed opvolgt.

> Je zult docker nodig hebben op je apparaat om met de containers te kunnen werken, mocht je deze nog niet hebben geinstalleerd kun je de installer hier vinden [Docker website].

[docker website]: https://docs.docker.com/get-docker/

Om snel met deze Dockerfile en database te werken, heb je ook een GUI voor databases nodig, zoals **MySQL Workbench, DBeaver, HeidiSQL of Navicat**. Installeer 1 van deze GUI\`s en ga dan verder.
Hier is een Dockerfile om een MySQL-database op te zetten, zet deze docker-file in de root van je project met als bestandsnaam `Dockerfile`:

```perl
# Use the official MySQL Docker image as base
FROM mysql:latest

# Enviroment variables of database
ENV MYSQL_ROOT_PASSWORD=<my-secret-pw>
ENV MYSQL_DATABASE=<my-db-name>
ENV MYSQL_USER=<my-user>
ENV MYSQL_PASSWORD=<my-password>

# Expose MySQL-port
EXPOSE 3306
```

Deze Dockerfile maakt gebruik van de officiële MySQL Docker image als basis en configureert omgevingsvariabelen voor de gebruikersnaam, wachtwoord, root wachtwoord en de naam van de database. Het kopieert ook alle SQL-scripts die zich in de scripts map bevinden naar de /docker-entrypoint-initdb.d/ map in de container, zodat ze automatisch worden uitgevoerd wanneer de container wordt gestart. De standaard MySQL-port is 3306.

Bouw de Docker-image met behulp van het volgende command:

```bash
docker build -t my-mysql-image .
```

Hiermee bouw je een Docker-image met de naam 'my-mysql-image'.

Start een nieuwe MySQL-container met het volgende command:

```bash
docker run --name my-mysql-container -p 3306:3306 -d my-mysql-image
```

Hiermee start je een nieuwe MySQL-container met de naam 'my-mysql-container', met portforwarding van de hostmachine-port 3306 naar de container-port 3306. De container draait nu op de achtergrond en is klaar voor gebruik.

Je kunt de docker-container stoppen met behulp van het **docker stop** command.
Voer het volgende command uit om een lijst met actieve containers te krijgen:

```bash
docker ps
```

Zoek de container die je wilt stoppen en kopieer de container ID.
Stop de container met behulp van het **docker stop** command, gevolgd door de container ID:

```bash
docker stop <container-ID>
```

Hiermee wordt de container gestopt en beëindigd.
Als je de container later weer wilt starten, kun je het volgende command gebruiken:

```bash
docker start my-mysql-container
```

> Gebruik nu de GUI met je gedefineerde inlog codes om een database aan te maken.

#### Lees meer

- [Jsonwebtoken website](https://github.com/auth0/node-jsonwebtoken)
- [Middleware functies](https://expressjs.com/en/guide/using-middleware.html)
- [Docker commands](https://dockerlabs.collabnix.com/docker/cheatsheet/)
