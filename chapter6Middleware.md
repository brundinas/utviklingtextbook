### Hva er Middleware?

**Middleware** refererer til funksjoner som har tilgang til **request**-objektet (`req`), **response**-objektet (`res`), og **next**-funksjonen i applikasjonens forespørsels-syklus. Middleware fungerer som et lag mellom forespørsler og svar, som lar deg håndtere data, utføre logikk, validere brukere, og mye mer, før du sender en endelig respons.

#### Hvordan fungerer middleware?
Når en HTTP-forespørsel kommer inn, passerer den gjennom en kjede av middleware-funksjoner før den når en sluttfunksjon som sender et svar tilbake til klienten. Hver middleware kan utføre noe arbeid og deretter enten:


### Typer av Middleware
I Express finnes det flere typer middleware, avhengig av hva de gjør og hvordan de settes opp.

Under ser du et eksempel på hvordan du kan bruke middleware i Express: Funksjonen `checkAuth` sjekker om en bruker er logget inn før de får tilgang til en spesifikk rute. Hvis brukeren er logget inn, går forespørselen videre til neste middleware, f.eks checkRole Hvis ikke, omdirigeres brukeren til innloggingssiden.

```javascript

### Bruk av Middleware for Ulike Formål

1. **Autentisering**
   Du kan bruke middleware for å kontrollere om en bruker er logget inn før de får tilgang til visse ruter.
   ```javascript
   function checkLoggedIn(req, res, next) {
       if (req.session && req.session.loggedIn) {
           next(); // Bruker er loggetinn, gå videre
       } else {
           res.redirect('/login'); // Ikke autentisert, omdiriger
       }
   }

   function checkSomethingElse(req, res, next) {
       if (req.userid && getUser(req.userid).role === 'admin') {
           next(); // Bruker er autentisert, gå videre
       } else {
           res.redirect('/dasboard'); // Ikke autentisert, omdiriger
       }
   }

   app.get('/dashboard', checkLoggedIn, (req, res) => {
       res.send('Velkommen til ditt dashbord!');

   app.get('/admin', checkLoggedIn, checkSomethingElse (req, res) => {
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