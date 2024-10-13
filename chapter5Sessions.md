
```markdown
# Working with Sessions in Node.js

In this chapter, we will look at how to store information about a user in a Node.js application session. This is particularly useful for:
- Storing whether a user is logged in or not
- Saving settings a user has applied, such as preferences in a shopping cart

First, let's see how we can use sessions in practice and understand what they are doing.

![Figure 1-1](https://raw.github.com/brundinas/utviklingtextbook/img/sessions.jpg "Sessions")

## Example 1 - Saving Visit Count

We want to store and display how many times a user has visited our page. Initially, we can use something as simple as this:

```javascript
let visitsCounter = 0;
app.get('/visits', (request, response) => {
    visitsCounter++;
    response.send("Total visits: " + visitsCounter);
});
```

### Problem

The issue with this solution is that it only tracks the **total** number of visits, not individual visits for each user. To solve this, we can install the **express-session** package.

```bash
npm install express-session
```

### Using express-session

We configure the express-session as follows:

```javascript
const session = require('express-session');
app.use(session({
    secret: 'Keep it secret',
    resave: false,
    saveUninitialized: false
}));
```

We now have access to a session object in the request handler. We can store session-specific variables for each user. For example, we can track how many visits a particular user has made:

```javascript
app.get('/visits', (request, response) => {
    if (request.session.visitsCounter === undefined) {
        request.session.visitsCounter = 1;
    } else {
        request.session.visitsCounter += 1;
    }
    response.send("Visits: " + request.session.visitsCounter);
});
```

In this code, we are now tracking each user’s number of visits using the `request.session.visitsCounter` variable.

## Example 2 - Logging In

In the first example, we stored how many times a user has visited. Now, let’s extend this by tracking whether a user is logged in or not.

We can create a simple variable `session.loggedIn` to manage the login state:

```javascript
session.loggedIn = true;
```

Or to log out:

```javascript
session.loggedIn = false;
```

### HTML Form for Logging In

We can create a simple HTML form to log in:

```html
<form action="/login" method="POST">
    <label for="username">Username:</label>
    <input type="text" name="username" id="username">
    <label for="password">Password:</label>
    <input type="password" name="password" id="password">
    <button type="submit">Login</button>
</form>
```

### Handling Login Logic

Here is an example of how to handle the login in the server code:

```javascript
const myPassword = "123";

app.post('/login', (request, response) => {
    if (request.body.username === 'myusername' && request.body.password === myPassword) {
        request.session.loggedIn = true;
        response.redirect('/');
    } else {
        request.session.loggedIn = false;
        response.redirect('/loginForm.html');
    }
});
```

Now, when a user submits the form, the server checks if the username and password match the expected values. If they do, the user is logged in.

### Requiring Login for Pages

On pages that require the user to be logged in, we can add this simple logic in the route handler:

```javascript
if (request.session.loggedIn !== true) {
    response.redirect('/loginForm.html');
    return;
}
```

Instead of storing whether a user is logged in, you could also store the username in `session.username` for later use.

## How Sessions Work

When a user visits a website, they send a request to the server with their address in the URL. This communication completes when the server responds, and by default, the server doesn’t know if the same person visits again.

Sessions solve this by recognizing a user between requests. This is made possible using **cookies**, small bits of data that the server sends back with the website and that the browser stores. On subsequent visits, the browser sends these cookies back to the server to identify the user.

With **express-session**, the server sends back a unique session ID, which is stored in the cookie. On the next request, this session ID is sent back, allowing the server to identify the user and retrieve their session data.

### How It Works Step by Step:
1. A user enters a URL to access the server.
2. The server processes the request and creates a session with a unique session ID.
3. The website is displayed, and the session ID is stored in a cookie.
4. When the user revisits the page, the session ID is sent back, and the server recognizes the user.
5. The server responds with personalized information based on the user’s previous interactions.
```

