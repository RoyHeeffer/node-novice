# EJS-templates

Embedded JavaScript, of EJS, is een template-engine voor Node.js die het mogelijk maakt om dynamische HTML-pagina's te genereren. Het werkt door het insluiten van JavaScript-code in HTML-bestanden. Lees hier meer over in de [docs].

## Kort voorbeeld

Hieronder vind je een kort voorbeeld hoe je deze kan gebruiken:

```javascript
const express = require("express");
const app = express();
const path = require("path");

// Set the view engine and view folder
app.set("view engine", "ejs");
app.set("views", path.join(__dirname, "views"));

// Render a view template
app.get("/", (req, res) => {
  res.render("home", { title: "Homepage", message: "Welcome to my blog!" });
});

app.listen(3000, () => {
  console.log("Server started on port 3000");
});
```

Hier wordt de **view engine** ingesteld op `EJS` en wordt de map waarin de EJS-bestanden zich bevinden ingesteld als de views directory. Vervolgens wordt er een route-handler aangemaakt voor de root van de website en wordt het bestand `index.ejs` gerenderd met de gegeven variabelen title en message. In het EJS-bestand kunnen deze variabelen vervolgens worden gebruikt door ze tussen `<%= %>` tags te plaatsen:

```html
<!-- views/home.ejs -->

<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
  </head>
  <body>
    <h1><%= message %></h1>
  </body>
</html>
```

Dit voorbeeld zal een HTML-pagina renderen met als titel **"Homepage"** en een h1-tag met de tekst **"Welcome to my blog!"**.

## Templates

Hieronder vind je wat simpele, niet gestylde templates om je snel op weg te helpen:

```html
<!-- views/home.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Blog Home</title>
  </head>
  <body>
    <h1>Welcome to the Blog!</h1>
    <ul>
      <% posts.forEach(function(post) { %>
      <li>
        <a href="/post/<%= post.id %>"><%= post.title %></a>
        <p><%= post.content %></p>
        <p>Author: <%= post.author.name %></p>
      </li>
      <% }); %>
    </ul>
  </body>
</html>
```

```html
<!-- views/login.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Login</title>
  </head>
  <body>
    <h1>Login to the Blog</h1>
    <% if (error) { %>
    <p><%= error %></p>
    <% } %>
    <form action="/login" method="POST">
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" /><br /><br />
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" /><br /><br />
      <button type="submit">Login</button>
    </form>
  </body>
</html>
```

```html
<!-- views/newPost.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Create a New Post</title>
  </head>
  <body>
    <h1>Create a New Post</h1>
    <% if (error) { %>
    <p><%= error %></p>
    <% } %>
    <form action="/post/new" method="POST">
      <label for="title">Title:</label>
      <input type="text" id="title" name="title" /><br /><br />
      <label for="content">Content:</label>
      <textarea id="content" name="content"></textarea><br /><br />
      <button type="submit">Create Post</button>
    </form>
  </body>
</html>
```

```html
<!-- views/editPost.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Edit Post</title>
  </head>
  <body>
    <h1>Edit Post</h1>
    <% if (error) { %>
    <p><%= error %></p>
    <% } %>
    <form action="/post/<%= post.id %>/edit" method="POST">
      <label for="title">Title:</label>
      <input
        type="text"
        id="title"
        name="title"
        value="<%= post.title %>"
      /><br /><br />
      <label for="content">Content:</label>
      <textarea id="content" name="content"><%= post.content %></textarea
      ><br /><br />
      <button type="submit">Update Post</button>
    </form>
  </body>
</html>
`
```

[docs]: https://ejs.co/#docs
