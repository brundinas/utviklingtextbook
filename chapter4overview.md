#Oversikt over hvordan express og sessions fungerers

### 1. **Express**
   **Express** er et minimalt og fleksibelt **webapplikasjonsrammeverk for Node.js** som tilbyr funksjoner for å bygge webapplikasjoner og API-er. Det brukes ofte til å lage server-side-applikasjoner som håndterer forespørsler og svar i en webapplikasjon.

   - **Express gjør det enklere å lage ruter, håndtere HTTP-metoder (GET, POST, osv.), administrere middleware, og koble til databaser eller autentiseringstjenester.
 

### 2. **Session**
   **Session** refererer til en måte å lagre informasjon om en brukers interaksjon med en webapplikasjon på tvers av flere forespørsler. I webutvikling opprettes det en session for hver bruker, og denne lagrer informasjon som bevares mellom forskjellige sideforespørsler (f.eks. om en bruker er logget inn, handlekurv, osv.).

   - **Hvordan det fungerer**: Sessioner er essensielle for å opprettholde brukerens tilstand mellom HTTP-forespørsler. Siden HTTP er stateless, gjør sesjoner det mulig for webapplikasjoner å følge brukerens handlinger og data på tvers av flere interaksjoner.
   - **Hvordan det fungerer**: Vanligvis lagres en session på serveren, og brukerens nettleser mottar en sesjons-ID i en cookie som sendes med hver forespørsel.

### 3. **express-session**
   **`express-session`** er et mellomvare for **Express** som gjør det mulig å administrere sesjoner i webapplikasjoner. Det lar deg lagre data på serveren og koble disse til individuelle brukere via en sesjons-ID lagret i en cookie. På denne måten kan en brukers tilstand opprettholdes over flere forespørsler.

   - **Hvordan det fungerer**: Når en session opprettes, genererer mellomvaren en sesjons-ID og sender den til brukerens nettleser som en cookie. Ved fremtidige forespørsler sender nettleseren denne sesjons-ID-en tilbake til serveren, som da henter den tilhørende sesjonsdataen.

   - **Eksempel på bruk**: Du kan bruke `express-session` til å lagre om en bruker er logget inn, hvilke varer som er i handlekurven, eller for å spore brukerpreferanser på nettstedet.

### 4. **Session i Express**
   I **Express** administreres sessioner vanligvis ved hjelp av `express-session` mellomvaren. Sesjonsdataen lagres på serveren, og sesjons-ID-en lagres på klientsiden i en cookie. Serveren sjekker sesjons-ID-en ved hver forespørsel for å hente lagrede data for den brukeren.

   - **Sessions i Express tillater**:
     - Lagring av innloggingsstatus (f.eks. om en bruker er logget inn).
     - Holde oversikt over brukeraktivitet på tvers av flere sidebesøk.
     - Lagring av midlertidig informasjon som skjemaopplysninger eller brukerpreferanser.




### Hvordan disse konseptene fungerer sammen:
1. **Express** gir rammene for å bygge webapplikasjoner og administrere HTTP-forespørsler og svar.
2. **Sessions** (via `express-session`) brukes til å administrere brukerens tilstand mellom forespørsler (f.eks. om brukeren er logget inn).

4. **Sessions i Express** brukes til å koble brukerens nettleser til deres sesjonsdata lagret på serveren (ofte i en database eller i minnet).

Denne kombinasjonen av verktøy brukes ofte til å utvikle små til mellomstore webapplikasjoner. Express håndterer serverlogikken, sesjoner holder oversikt over brukerne, og SQLite administrerer datalagring.