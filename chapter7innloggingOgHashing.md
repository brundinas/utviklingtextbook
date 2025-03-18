# Kapittel 7: Innlogging og hashing

# Eksempel: Sikker innlogging med sessions og bcrypt

Dette eksempelet viser hvordan du kan lage et enkelt innloggingssystem med JavaScript (Node.js) og Express. Vi bruker `express-session` for å holde brukeren innlogget, og `bcrypt` for å sikre passordene ved å hashe og salte dem før de lagres i databasen.

## 1. HTML-skjema for innlogging

Først lager vi et HTML-skjema som sender brukernavn og passord til serveren når brukeren logger inn.

```html
<form action="/login" method="POST">
  <label for="username">Brukernavn:</label>
  <input type="text" id="username" name="username" required>
  
  <label for="password">Passord:</label>
  <input type="password" id="password" name="password" required>
  
  <button type="submit">Logg inn</button>
</form>
```

Dette skjemaet sender en POST-forespørsel til `/login`-ruten på serveren med brukernavn og passord.

## 2. Serverkode med Express og bcrypt

For å håndtere innloggingen, trenger vi en server som verifiserer brukernavn og passord mot en database. Passordet er lagret som en hash for økt sikkerhet.

### Installasjon av nødvendige pakker:

```bash
npm install express express-session bcrypt
```

### Konfigurasjon av Express og session:

```javascript
const express = require('express');
const session = require('express-session');
const bcrypt = require('bcrypt');
const app = express();

// Middleware for å parse innkommende forespørsler
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

// Konfigurere session
app.use(session({
    secret: 'hemmelig_nøkkel',
    resave: false,
    saveUninitialized: true,
    cookie: { secure: false } // Sett til true hvis du bruker HTTPS
}));

// Simulerer en database av brukere med hash-verdi for passord
// Legg inn denne hashete passordet for en av brukeree i databasen 
// password: '$2b$10$OaYrsjfSOxIlRl3l6brlTe4erojrTxjgsYSzUNF.uCa9Ny9XMmXoS' 
// Hash av "Passord123"

// Hashing av nytt passord (kan brukes for å opprette brukere)
//    const saltRounds = 10;
//    const hashedPassword = await bcrypt.hash(password, saltRounds);
```

### 3. Håndtering av innlogging med bcrypt

Når brukeren sender inn skjemaet, sammenligner vi det oppgitte passordet med den lagrede hashen ved bruk av `bcrypt.compare()`.

```javascript
// Rute for innlogging
app.post('/login', async (req, res) => {
    const { username, password } = req.body;
    
    // Finn brukeren basert på brukernavn
    const sql = db.prepare('SELECT password FROM user where username =?') 
    const user = sql.all(username)   
    if (!user) {
        return res.status(401).send('Ugyldig brukernavn eller passord');
    }

    // Sjekk om passordet samsvarer med hash'en i databasen
    const isMatch = await bcrypt.compare(password, user.password);

    if (isMatch) {
        // Lagre innloggingsstatus i session
        req.session.loggedIn = true;
        req.session.username = user.username;
        return res.send('Innlogging vellykket!');
    } else {
        return res.status(401).send('Ugyldig brukernavn eller passord');
    }
});

// Beskyttet rute som krever at brukeren er innlogget
app.get('/dashboard',  checkLoggedIn, (req, res) => {
          res.redirect("/dashboard.html"); // Redirect on successful login
});

function checkLoggedIn(req, res, next) {
    if (req.session && req.session.loggedIn) {
        next(); // Bruker er loggetinn, gå videre
    } else {
        res.redirect('/login'); // Ikke autentisert, omdiriger
    }
}

```

### Forklaring av koden:
1. **Innlogging**: Når brukeren sender inn sitt brukernavn og passord, sjekker serveren først om brukernavnet eksisterer. Deretter bruker den `bcrypt.compare()` til å sammenligne det innsendte passordet med den lagrede hash'en.
   
2. **Session**: Hvis brukeren har oppgitt riktig passord, opprettes en session med `req.session.loggedIn = true`. Dette betyr at brukeren forblir innlogget på serveren til de logger ut eller sesjonen utløper.

3. **Beskyttet rute**: Ruten `/dashboard` krever at brukeren er innlogget for å få tilgang. Hvis brukeren ikke er innlogget, omdiriges brukeren til innloggings siden.

### 4. Opprettelse av brukere med hashing

Når du oppretter nye brukere, er det viktig å hashe passordet deres før det lagres i databasen. Dette forhindrer at passord blir lagret i klartekst.

```javascript
// Eksempel på hvordan du kan hashe et nytt passord når du oppretter en bruker
createUser('nybruker', 'mittPassord123');
```

Dette vil lagre en ny bruker med brukernavnet `nybruker` og et hashet passord i `users`-arrayen.

### 5. Sammenligning av passord

Når en bruker prøver å logge inn, bruker vi `bcrypt.compare()` for å sammenligne det oppgitte passordet med det hashede passordet som er lagret i databasen.

```javascript
const isMatch = await bcrypt.compare(password, user.password);
```

Hvis passordet samsvarer, logges brukeren inn. Hvis det ikke samsvarer, vil en feilmelding returneres.

### 6. Logge ut brukeren

Du kan enkelt logge ut brukeren ved å ødelegge sesjonen:

```javascript
app.get('/logout', (req, res) => {
    req.session.destroy((err) => {
        if (err) {
            return res.status(500).send('Feil under utlogging');
        }
        res.send('Du er nå logget ut.');
    });
});
```

---

### Oppsummering:

Dette innloggingseksempelet viser hvordan man kan lage en enkel, men sikker innloggingsløsning med:
- **HTML-skjema** for å sende brukernavn og passord.
- **Sessions** for å holde brukeren innlogget.
- **bcrypt** for hashing og validering av passord.

Ved å bruke sessions og bcrypt kan vi sikre at brukerne forblir autentisert på en sikker måte, samtidig som vi beskytter deres passord mot angrep.