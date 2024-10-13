### Hva er Middleware i Webutvikling?

**Middleware** i webutvikling, og spesielt i **Express**, refererer til funksjoner som har tilgang til **request**-objektet (`req`), **response**-objektet (`res`), og **next**-funksjonen i applikasjonens forespørsels-syklus. Middleware fungerer som et lag mellom forespørsler og svar, som lar deg håndtere data, utføre logikk, validere brukere, og mye mer, før du sender en endelig respons.

#### Hvordan fungerer middleware?
Når en HTTP-forespørsel kommer inn, passerer den gjennom en kjede av middleware-funksjoner før den når en sluttfunksjon som sender et svar tilbake til klienten. Hver middleware kan utføre noe arbeid og deretter enten:
- **Stoppe** kjeden og sende et svar (for eksempel ved å returnere en 404-feil hvis ressursen ikke finnes), eller
- **Kalle `next()`**, som gir stafettpinnen videre til neste middleware i kjeden.

### Typer av Middleware
I Express finnes det flere typer middleware, avhengig av hva de gjør og hvordan de settes opp.

#### 1. **Brukergenerert (applikasjonsspesifikk) middleware**
   Dette er tilpassede middleware-funksjoner som du definerer i applikasjonen for å utføre spesifikke oppgaver, som logging, autentisering, validering, eller manipulering av forespørselsdata.

   **Eksempel**:
   ```javascript
   const express = require('express');
   const app = express();

   // Egendefinert middleware for logging
   app.use((req, res, next) => {
       console.log(`${req.method} request for ${req.url}`);
       next(); // Gå videre til neste middleware
   });

   app.get('/', (req, res) => {
       res.send('Hello, world!');
   });

   app.listen(3000, () => {
       console.log('Server running on port 3000');
   });
   ```
   I dette eksemplet logger middleware hver innkommende forespørsel og gir deretter kontroll videre til neste middleware eller rute ved å kalle `next()`.

#### 2. **Innebygd middleware**
   Express har flere innebygde middleware-funksjoner som kan brukes til vanlige oppgaver, som å tjene statiske filer eller parsing av innkommende data.

   **Eksempler på innebygd middleware**:
   - **express.static**: Tjener statiske filer som HTML, CSS, bilder osv.
     ```javascript
     app.use(express.static('public'));
     ```
   - **express.json**: Brukes til å analysere JSON-data fra innkommende forespørsler.
     ```javascript
     app.use(express.json());
     ```

#### 3. **Tredjeparts middleware**
   Det finnes mange tredjeparts middleware-pakker tilgjengelig via npm som du kan bruke til å utvide funksjonaliteten til applikasjonen din. Eksempler inkluderer autentisering, logging, sikkerhet, og mer.

   - **Morgan** (for logging av forespørsler):
     ```javascript
     const morgan = require('morgan');
     app.use(morgan('dev'));
     ```
   - **express-session** (for håndtering av sesjoner):
     ```javascript
     const session = require('express-session');
     app.use(session({
         secret: 'my-secret',
         resave: false,
         saveUninitialized: true
     }));
     ```

### Bruk av Middleware for Ulike Formål

1. **Autentisering**
   Du kan bruke middleware for å kontrollere om en bruker er logget inn før de får tilgang til visse ruter.
   ```javascript
   function checkAuth(req, res, next) {
       if (req.session && req.session.loggedIn) {
           next(); // Bruker er autentisert, gå videre
       } else {
           res.redirect('/login'); // Ikke autentisert, omdiriger
       }
   }

   app.get('/dashboard', checkAuth, (req, res) => {
       res.send('Velkommen til ditt dashbord!');
   });
   ```

2. **Feilhåndtering**
   Feilhåndteringsmiddleware er en type middleware som kun blir kalt hvis det oppstår en feil i applikasjonen. Dette kan brukes til å logge feil og sende tilpassede feilmeldinger.
   ```javascript
   app.use((err, req, res, next) => {
       console.error(err.stack);
       res.status(500).send('Noe gikk galt!');
   });
   ```

3. **Logging**
   Middleware kan brukes til å logge innkommende forespørsler, inkludert tidspunkt, metode, og URL.
   ```javascript
   app.use((req, res, next) => {
       console.log(`${new Date().toISOString()} - ${req.method} request to ${req.url}`);
       next();
   });
   ```

### Oppsummering av Middleware:
- **Middleware** er funksjoner som har tilgang til forespørselen (request), svaret (response), og funksjonen `next()` i Express-applikasjoner.
- De kan brukes til:
  - Å utføre handlinger på data i forespørsler.
  - Å håndtere autentisering, logging, feilhåndtering, og andre oppgaver.
  - Å kontrollere flyten av applikasjonen ved å enten sende et svar eller kalle `next()` for å gå videre til neste middleware.
- Middleware kan være:
  - **Brukergenerert** (tilpassede funksjoner).
  - **Innebygd** (for vanlige oppgaver som statiske filer eller JSON-parsing).
  - **Tredjeparts** (via npm-pakker for spesifikke formål som logging eller sesjoner).

Middleware er et kraftig verktøy som gjør det mulig å bygge modulære og skalerbare applikasjoner ved å dele opp logikken i håndterbare biter som hver har sitt spesifikke ansvar.