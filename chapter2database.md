#Database
Vi benytter SQLite som database. 
SQLite studio er et frontend verktøy for å kjøre sql, endre tabeller. 
Den kan lastes ned herfra: https://www.sqlitestudio.pl/

### 1. **Datatyper i SQLite** (med DATETIME)

Vanlige datatyper:
- **INTEGER**: Heltall (brukes ofte for primærnøkler).
- **TEXT**: Tekst (streng).
- **REAL**: Desimaltall (flyttall).
- **BLOB**: Binærdata.
- **NULL**: Null-verdi.
- **DATETIME**: Brukes for å lagre dato og tid, selv om dette teknisk sett lagres som **TEXT** eller **INTEGER** i SQLite.

   - **Eksempel**: 
     - `DATETIME('now')` vil lagre gjeldende dato og tid.
     - Du kan lagre en dato som tekst i formatet `YYYY-MM-DD HH:MM:SS`, som er det vanligste i SQLite.

### 2. **Primærnøkkel (PRIMARY KEY)**
En **primærnøkkel** er en unik identifikator for hver rad i en tabell. I SQLite er **INTEGER PRIMARY KEY**-kolonner spesielle, da de automatisk blir autoincrementerende, noe som betyr at verdien økes automatisk for hver ny rad.

#### Eksempel: Primærnøkkel i en tabell
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE
);
```

I dette eksempelet er `id` kolonnen definert som en **primærnøkkel** som vil autoincrementeres for hver ny bruker som legges til.

### 3. **Fremmednøkkel (FOREIGN KEY)**
En **fremmednøkkel** brukes til å koble sammen to tabeller basert på relasjonen mellom dem. Den refererer til en primærnøkkel i en annen tabell.

- Fremmednøkler brukes for å opprettholde referensiell integritet, noe som betyr at verdiene i fremmednøkkelfeltet må samsvare med verdier i den refererte tabellens primærnøkkel.

#### Eksempel: Fremmednøkkel
```sql
CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY AUTOINCREMENT,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    user_id INTEGER,
    FOREIGN KEY(user_id) REFERENCES users(id)
);
```

Her refererer `user_id` i `orders`-tabellen til `id`-kolonnen i `users`-tabellen. Dette betyr at hver bestilling (`order`) er knyttet til en bestemt bruker.

### 4. **SQL-Operasjoner med Eksempler**

#### a) **CREATE TABLE**
Brukes til å opprette en ny tabell, inkludert primær- og fremmednøkler.
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

Her brukes `DATETIME` til å lagre opprettelsestidspunktet for en bruker, og `AUTOINCREMENT` sikrer at hver ny rad får en unik ID.

#### b) **INSERT INTO**
Brukes til å sette inn data i en tabell, inkludert `DATETIME`-felt.
```sql
INSERT INTO users (username, email)
VALUES ('JaneDoe', 'jane@example.com');
```

Dette setter inn en ny bruker. `created_at` vil automatisk settes til gjeldende tidspunkt på grunn av `DEFAULT CURRENT_TIMESTAMP`.

#### c) **SELECT**
Henter data fra en eller flere tabeller.

##### Enkelt SELECT-spørring:
```sql
SELECT * FROM users;
```

##### SELECT med JOIN:
Hvis du vil hente brukerinformasjon sammen med deres bestillinger (relatert via fremmednøkkelen):
```sql
SELECT users.username, orders.order_date
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

Dette henter brukernavn fra `users`-tabellen og tilhørende `order_date` fra `orders`-tabellen ved hjelp av fremmednøkkelen `user_id`.

#### d) **UPDATE**
Brukes til å oppdatere eksisterende data.
```sql
UPDATE users
SET email = 'newemail@example.com'
WHERE id = 1;
```

Dette oppdaterer e-posten til brukeren med `id` lik 1.

#### e) **DELETE**
Sletter en rad fra en tabell.
```sql
DELETE FROM users WHERE id = 1;
```

Dette sletter brukeren med `id` lik 1.

#### f) **ALTER TABLE**
Brukes til å endre strukturen til en tabell, som å legge til kolonner eller fremmednøkler.

##### Legge til en kolonne:
```sql
ALTER TABLE users ADD COLUMN phone TEXT;
```

Dette legger til en ny kolonne `phone` til `users`-tabellen.

#### g) **DROP**
Brukes til å slette tabeller eller objekter i databasen.

##### Slette en tabell:
```sql
DROP TABLE users;
```

Dette sletter tabellen `users` og alle dataene i den.


### Oppsummering:

- **Primærnøkkel**: En unik identifikator for hver rad. Brukes med `INTEGER PRIMARY KEY AUTOINCREMENT` for automatisk økning av ID.
- **Fremmednøkkel**: Kobler sammen tabeller og refererer til primærnøkkelen i en annen tabell. Brukes til å opprettholde relasjoner og referensiell integritet.
- **DATETIME**: Brukes til å lagre dato og tid i et format som `YYYY-MM-DD HH:MM:SS` eller som Unix-tid. Kan settes automatisk med `DEFAULT CURRENT_TIMESTAMP`.
- **Viktige operasjoner**:
  - **CREATE TABLE**: Oppretter en ny tabell med kolonner, primærnøkler og fremmednøkler.
  - **INSERT INTO**: Legger til nye rader i tabellen.
  - **SELECT**: Henter data, ofte ved å bruke JOIN for å kombinere flere tabeller.
  - **UPDATE**: Endrer eksisterende data i tabellen.
  - **DELETE**: Fjerner data fra tabellen.
  - **ALTER TABLE**: Endrer strukturen til en eksisterende tabell.
  - **DROP**: Sletter en tabell eller kolonne.
 

