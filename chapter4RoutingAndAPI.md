# Kapittel 4: Routing and API

## Routing i Express

Express lar deg definere ruter for ulike HTTP-metoder som GET, POST, PUT og DELETE.


## Backend
Først må du opprette filen server.js/app. Den fungerer som en backend for applikasjonen/nettsiden/webappen. 
Her er et eksempel på en grunnleggende express server.

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json()); // Middleware for JSON-parsing

app.listen(PORT, () => {
    console.log(`Server kjører på http://localhost:${PORT}`);
});
```


## Routing i Express

Express lar deg definere ruter for ulike HTTP-metoder som GET, POST, PUT og DELETE.
```javascript
app.get('/', (req, res) => {
    res.send('Velkommen');
});

app.get('/about', (req, res) => {
    res.send('Dette er en enkel Express-app.');
});

//dynamisk ruting med parameter
app.get('/user/:id', (req, res) => {
    const userId = req.params.id;
    res.send(`Bruker-ID: ${userId}`);
});

//håndtering av post
app.post('/user', (req, res) => {
    const { name, age } = req.body;
    res.json({ message: `Bruker ${name} er ${age} år gammel.` });
});
```

## Eksempel ppsett av et RESTful API  uten database

```javascript
//typisk vil man lagre data i en database, men er et basic utgangspunkt
const users = [];

// Hent alle brukere
app.get('/users', (req, res) => {
    res.json(users);
});

// Legg til en ny bruker
app.post('/users', (req, res) => {
    const user = req.body;
    users.push(user);
    res.status(201).json({ message: 'Bruker lagt til', user });
});

// Oppdater en bruker
app.put('/users/:id', (req, res) => {
    const { id } = req.params;
    const updatedData = req.body;
    users[id] = { ...users[id], ...updatedData };
    res.json({ message: 'Bruker oppdatert', user: users[id] });
});

// Slett en bruker
app.delete('/users/:id', (req, res) => {
    const { id } = req.params;
    users.splice(id, 1);
    res.json({ message: 'Bruker slettet' });
});
```


## Eksempel oppsett av et RESTful API  med database

```javascript
//typisk vil man lagre data i en database, men er et basic utgangspunkt
const sqlite3 = require('better-sqlite3')
db  = sqlite3('./app.db', {verbose: console.log})

// Hent alle brukere
app.get('/users', (req, res) => {
    const sql = db.prepare('SELECT user.id as userid, firstname, lastname, email FROM user') 
    const users = sql.all()   
    res.send(users)
});

// Legg til en ny bruker
app.post('/adduser', (req, res) => {
    const user = req.body;
    sql = db.prepare("INSERT INTO user (firstName, lastName, email) VALUES (?, ?, ?)")
    const info = sql.run(user.firstName, user.lastName, user.email)
    console.log(info.lastInsertRowid)  
    res.status(201).json({ message: 'Bruker lagt til', user });
});

// Oppdater en bruker
app.put('/users/:id', (req, res) => {
    const { id } = req.params;
    const updatedData = req.body;
    sql = db.prepare("UPDATE user set firstName=?, lastName=?, email=? WHERE id = ? ")
    const info = sql.run(updatedData.firstName, updatedData.lastName, updatedData.email, id)
    res.json({ message: 'Bruker oppdatert', user: updatedData });
});

// Slett en bruker
app.delete('/users/:id', (req, res) => {
    const { id } = req.params;
    sql = db.prepare("DELETE FROM user WHERE id = ? ")
    const info = sql.run( id)

    res.json({ message: 'Bruker slettet' });
});
```