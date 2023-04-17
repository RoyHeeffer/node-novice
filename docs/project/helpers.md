# Helpers

Deze simpele helpers zijn bedoeld om je snel op weg te helpen, bekijk de functies goed en gebruik de documentatie om een beter begrip van deze functies te krijgen en waar nodig te verbeteren:

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

Documentatie [jsonwebtoken] library en [hier] vind je meer over express middleware functies.

[jsonwebtoken]: https://github.com/auth0/node-jsonwebtoken
[hier]: https://expressjs.com/en/guide/using-middleware.html
