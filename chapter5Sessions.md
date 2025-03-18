
```markdown
# Kapittel 5: Sessions i Node.js

I dette kapittelet skal vi se på hvordan vi kan lagre informasjon om en bruker i en økt (session) i en Node.js-applikasjon. Dette er spesielt nyttig for:

Å lagre om en bruker er logget inn eller ikke
Å lagre innstillinger en bruker har valgt, for eksempel preferanser i en handlekurv
Først skal vi se hvordan vi kan bruke økter i praksis og forstå hva de gjør.

![Figure 1-1](https://brundinas.github.io//utviklingtextbook//img//sessions.jpg)

## Eksempel 1 - Lagre en besøksteller

Vi ønsker å lagre og vise hvor mange ganger en bruker besøker netsiden vår. Dette kan gjøres så enkelt som dette:


```javascript
let visitsCounter = 0;
app.get('/visits', (request, response) => {
    visitsCounter++;
    response.send("Total visits: " + visitsCounter);
});
```

### Problem
Problemet her er at det lagrer totalt antall besøk, og ikke besøk ber bruker. For å løse dette, kan vi bruke **express-session**.

```bash
npm install express-session
```

### Bruk av express-session

We configure the express-session as follows:

```javascript
const session = require('express-session');
app.use(session({
    secret: 'Keep it secret',
    resave: false,
    saveUninitialized: false
}));
```
Vi har nå tilgang til et session-objekt i request-handleren. Vi kan lagre øktspesifikke variabler for hver bruker. For eksempel kan vi spore hvor mange ganger en bestemt bruker har besøkt siden:

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

## Hvordan sessions fungerer

Hvordan økter fungerer
Når en bruker besøker en nettside, sender de en forespørsel til serveren med sin adresse i URL-en. Denne kommunikasjonen avsluttes når serveren svarer, og som standard vet ikke serveren om den samme personen besøker siden igjen.

Økter løser dette ved å gjenkjenne en bruker mellom forespørsler. Dette gjøres mulig ved hjelp av cookies, små datastykker som serveren sender tilbake med nettsiden, og som nettleseren lagrer. Ved senere besøk sender nettleseren disse informasjonskapslene tilbake til serveren for å identifisere brukeren.

Med **express-session** sender serveren tilbake en unik session-ID, som lagres i en informasjonskapsel. Ved neste forespørsel sendes denne session-ID-en tilbake, slik at serveren kan gjenkjenne brukeren og hente deres øktdata.

### Slik fungerer det steg for steg:
- En bruker skriver inn en URL for å få tilgang til serveren.
- Serveren behandler forespørselen og oppretter en økt med en unik session-ID.
- Nettsiden vises, og session-ID-en lagres i en informasjonskapsel.
- Når brukeren besøker siden igjen, sendes session-ID-en tilbake, og serveren gjenkjenner brukeren.
- Serveren svarer med personlig tilpasset informasjon basert på brukerens tidligere interaksjoner.
```

