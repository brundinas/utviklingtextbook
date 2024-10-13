# Grunnleggende JavaScript

JavaScript er et kraftig programmeringsspråk som brukes til å lage dynamiske webapplikasjoner. I dette kapitlet vil vi se på noen grunnleggende konsepter i JavaScript, inkludert variabler, datatyper, kontrollstrukturer, funksjoner, og objekter. For å bli komfortabel med disse konseptene, anbefales det at du eksperimenterer med koden og ser hvordan den fungerer i praksis.


## Skriving til Console

En av de enkleste måtene å begynne å bruke JavaScript på er ved å skrive tekst til konsollen. Dette kan brukes til å teste koden din underveis.

```javascript
console.log("Hei!");
```

### Ulike typer variabler

I JavaScript kan du deklarere variabler ved å bruke `var`, `let`, eller `const`. Det er viktig å forstå forskjellene mellom disse.

- **`var`**: En eldre måte å deklarere variabler på. Den er funksjonsomfanget (ikke blokk-scope), noe som kan føre til uventede resultater.
- **`let`**: En nyere og foretrukket måte å deklarere variabler på. Den har blokk-scope, noe som betyr at den kun eksisterer i den blokken den er definert i.
- **`const`**: Brukes når du vil opprette en konstant verdi som ikke skal endres.

```javascript
var variabel = "Dette er en gammel type variabel";
let enNyVariabel = "Dette er en ny type variabel";
const enKonstant = "Variabel som ikke kan endres";

// enKonstant = "Nytt innhold"; // Feilmelding fordi enKonstant er konstant
```

### Tallvariabler og desimaltall

Tall (heltall og desimaltall) kan deklareres ved bruk av `let` eller `var`. Her er et eksempel:

```javascript
let tallVariabel = 1234;
tallVariabel = 1234.34;
```

Her setter vi først variabelen `tallVariabel` til et heltall, deretter endrer vi den til en desimalverdi.

#### Å øke med 1

Det er flere måter å øke en tallvariabel med 1 på:

```javascript
let tall = 1;
tall = tall + 1;
tall += 1;
tall++;
```

### Tabeller (Arrays)

En **array** i JavaScript er en samling av flere verdier lagret i én variabel.

```javascript
let liste = [1, 2, 3, 4];
console.log(liste[3]); // Skriver ut 4
```

I dette eksempelet oppretter vi en liste med tallene 1, 2, 3 og 4. Vi bruker `console.log` for å skrive ut det fjerde elementet i listen.

#### Flere eksempler på arrays:

```javascript
let tabell = ["Børge", "Garpestad"];
console.log(tabell[0]); // Skriver ut "Børge"
```

### Objekter

Et objekt er en datastruktur som kan inneholde flere verdier, ofte representert som egenskaper med navn.

```javascript
let person1 = {
    fornavn: "Hans",
    etternavn: "Hansen"
};

let person2 = {
    fornavn: "Truls",
    etternavn: "Trulsen"
};

let personer = [person1, person2];
console.log(personer[0].fornavn); // Skriver ut "Hans"
```

Her oppretter vi to objekter, `person1` og `person2`, som inneholder informasjon om to personer. Deretter legger vi dem inn i en array og skriver ut fornavnet til den første personen.

### If-setninger (True/False)

If-setninger brukes for å sjekke om en betingelse er sann eller usann.

```javascript
let sant = true;
sant = false;

let etTall = 3;
let endaEtTall = 3;
let svar = etTall * endaEtTall;

if (svar == 9) {
    console.log("Vi gikk inn i denne kodeblokken");
} else if (svar == 3) {
    console.log("Nei, denne blokken");
} else {
    console.log("Standardsvaret");
}
```

I dette eksempelet sjekker vi om verdien av `svar` er lik 9, og basert på det, utføres forskjellige handlinger.

### Løkker

Løkker er en effektiv måte å utføre en oppgave flere ganger. En **for-løkke** lar deg kjøre en kodeblokk et spesifikt antall ganger.

```javascript
for (let i = 0; i < 10; i++) {
    console.log(i); // Skriver ut tallene 0 til 9
}
```

Du kan også bruke løkker til å iterere gjennom en array:

```javascript
let dyrTabell = ["hest", "geit", "gris", "hjort"];
for (let i = 0; i < dyrTabell.length; i++) {
    console.log(dyrTabell[i]); // Skriver ut alle elementene i tabellen
}
```

### Funksjoner

Funksjoner er gjenbrukbare kodeblokker som kan ta inn parametere og utføre en oppgave.

#### Definere og bruke funksjoner

```javascript
function einFunksjon(fornavn, etternavn) {
    console.log("Hei ", fornavn, etternavn);
}

einFunksjon("Børge", "Garpestad"); // Kaller funksjonen
```

Denne funksjonen tar inn to parametere (`fornavn` og `etternavn`) og skriver dem ut til konsollen.

#### Funksjoner i løkker

```javascript
function kulFunksjon(navn) {
    console.log(navn, "er kul");
}

for (let i = 0; i < 4569; i++) {
    kulFunksjon("Børge");
}
```

Dette kaller funksjonen `kulFunksjon` 4569 ganger med navnet "Børge".

### Parametere og returverdier

Funksjoner kan også returnere verdier til den som kaller funksjonen:

```javascript
function getTotalLength(string1, string2) {
    let length = string1.length + string2.length;
    return length;
}

let totalLength = getTotalLength("Kasper", "Iversen");
console.log(totalLength); // Skriver ut lengden av begge strengene
```

I dette eksempelet returnerer funksjonen lengden på de to strengene som sendes inn som argumenter.

