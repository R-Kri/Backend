# Body Parser
```
    // middleware
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({extended: true}));
```
## 🧾 1. `app.use(bodyParser.json());`

### ✅ What it does:

* Tells Express to **parse incoming requests with a `Content-Type: application/json`** header.
* Converts the **raw JSON string in the request body** into a **JavaScript object** and attaches it to `req.body`.

### 🔍 Why it's needed:

When a client (like Postman, or a frontend app) sends JSON data:

```json
{
  "username": "alice",
  "password": "1234"
}
```

Without this middleware:

```js
console.log(req.body); // undefined
```

With this middleware:

```js
console.log(req.body); 
// { username: 'alice', password: '1234' }
```

This is essential for **handling API requests** that send JSON payloads (which is very common in REST APIs).

---

## 📝 2. `app.use(bodyParser.urlencoded({ extended: true }))`

### ✅ What it does:

* Parses incoming requests with **`Content-Type: application/x-www-form-urlencoded`** — like the data sent from **HTML forms**.
* Converts form data into a **JavaScript object** and attaches it to `req.body`.

For example, a form like:

```html
<form method="POST" action="/login">
  <input name="email" value="bob@example.com" />
  <input name="password" value="secret" />
</form>
```

Will send:

```
email=bob@example.com&password=secret
```

With this middleware:

```js
console.log(req.body); 
// { email: 'bob@example.com', password: 'secret' }
```

### 🔍 What's `extended: true`?

* When `true`: it uses the **qs library** under the hood, allowing for **nested objects and arrays** in the request body.
* When `false`: uses the **querystring library**, which handles only flat key-value pairs.

Example with `extended: true`:

```js
user[name]=John&user[age]=30
// becomes: { user: { name: 'John', age: 30 } }
```

With `extended: false`, you'd lose that structure.

---

## ⚠️ Modern alternative (for Express 4.16+):

If you're using **Express 4.16 or newer**, you **don’t need to install `body-parser` manually** — it’s built-in.

So instead of:

```js
const bodyParser = require('body-parser');
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
```

You can just do:

```js
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

---

## ✅ Summary

| Middleware                | Parses what                                     | Attaches to `req.body`     | Common use case                |
| ------------------------- | ----------------------------------------------- | -------------------------- | ------------------------------ |
| `bodyParser.json()`       | JSON (`application/json`)                       | `{ key: value }`           | REST APIs, JSON payloads       |
| `bodyParser.urlencoded()` | Form data (`application/x-www-form-urlencoded`) | `{ key: value }` or nested | HTML forms, login/signup forms |

