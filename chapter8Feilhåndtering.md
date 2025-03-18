# Kapittel 8: Feilhåndtering

### Eksempel: Feilhåndtering i Express
Feilhåndtering med middleware er en viktig del av utvikling med **Express**. En typisk feilbehandlingsmiddleware har tilgang til den samme informasjonen som vanlig middleware (request, response, og `next`), men i tillegg tar den imot en ekstra **error-parameter**. Dette lar den håndtere feil som oppstår under kjøring av andre middleware eller ruter.

Her er et konkret eksempel som viser hvordan du kan bruke **error-handling middleware** i Express:

### Eksempel: Feilhåndtering i Express

```javascript
const express = require('express');
const app = express();

// Vanlig rute som returnerer data
app.get('/', (req, res) => {
    res.send('Velkommen til hjemmesiden!');
});

// En rute som genererer en feil
app.get('/error', (req, res, next) => {
    const error = new Error('Noe gikk galt!');
    error.status = 500;
    next(error);  // Sender feilen videre til feilhåndteringsmiddleware
});

// Feilhåndteringsmiddleware
app.use((err, req, res, next) => {
    console.error(err.stack);  // Logger feilen til konsollen

    res.status(err.status || 500);  // Setter statuskode basert på feilen eller 500 (intern serverfeil)
    res.json({
        message: err.message,
        // Denne linjen legger til stack trace (fjerning av stack trace i produksjon kan være nødvendig for sikkerhet):
        error: err.stack 
    });
});

// Start serveren
app.listen(3000, () => {
    console.log('Server kjører på http://localhost:3000');
});
```

### Forklaring av koden:

1. **Ruter**:
   - `/`: Returnerer en velkomstmelding. Dette er en standard rute uten noen feil.
   - `/error`: Denne ruten genererer en feil ved å skape et nytt **Error**-objekt og setter statuskoden til 500. Deretter sender den feilen videre ved å kalle `next(error)`, noe som betyr at Express vil hoppe over vanlig middleware og gå rett til feilhåndteringsmiddleware.

2. **Feilhåndteringsmiddleware**:
   - Feilhåndteringsmiddleware er spesielt fordi det har fire parametere: **err**, **req**, **res**, og **next**. Dette gjør det mulig å fange opp feilen og sende en tilpasset respons til brukeren.
   - **Logging**: Feilen blir logget til konsollen med `console.error(err.stack)`, som gir en stack trace (nyttig for debugging).
   - **Respons**: Den setter HTTP-statuskoden til 500 (eller en annen kode som er spesifisert i feilen) og returnerer en JSON-respons som inkluderer feilmeldingen og en stack trace for å gi innsikt i hva som gikk galt. Merk: I en produksjonsapplikasjon kan du ønske å skjule stack trace fra brukeren for sikkerhetsgrunner.

3. **Test**:
   - Når du besøker `/error`, vil du utløse en feil som blir håndtert av feilhåndteringsmiddleware.
   - Når du besøker `/`, vil du se den vanlige velkomstmeldingen.

### Feilhåndtering i Produksjon

I en faktisk produksjonsapplikasjon er det vanlig å skjule sensitive detaljer, som stack trace, fra sluttbrukeren og heller vise en generisk feilmelding. Her er et eksempel på hvordan du kan endre feilhåndteringsmiddleware for produksjon:

```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);

    // Skjul stack trace for brukeren i produksjon
    res.status(err.status || 500);
    res.json({
        message: err.message,
        error: process.env.NODE_ENV === 'development' ? err.stack : {}
    });
});
```

- **`process.env.NODE_ENV`**: Hvis applikasjonen kjører i utviklingsmodus (`development`), viser den stack trace. Hvis den kjører i produksjon (`production`), vil stack trace være skjult for brukeren.

### Når Brukes Feilhåndtering?

- **Serverfeil**: Når noe uventet skjer i programmet, som en databasefeil, manglende fil, eller et annet problem.
- **Ruteproblemer**: Når en bruker forsøker å få tilgang til en rute som forårsaker feil, for eksempel en rute som kaster en spesifikk feil for testing.
- **Valideringsfeil**: Hvis brukeren sender feil data til serveren, kan en feilmelding returneres som respons, og den kan fanges opp med feilhåndtering middleware.

Feilhåndtering er en nøkkelkomponent i å gjøre applikasjonen robust og brukervennlig ved å sikre at brukerne får tilpassede og informative tilbakemeldinger når noe går galt.