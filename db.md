
1. Instalar el paquete sqlite3 en el proyecto:

```
npm install sqlite3
```

2. Crear un archivo database.js para configurar la BD:

```js
// database.js

const sqlite3 = require('sqlite3').verbose();
const db = new sqlite3.Database('./db/database.sqlite');

// crear tabla usuarios
const CREATE_USERS_TABLE = `
  CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT, 
    email TEXT UNIQUE, 
    password TEXT
  )
`;

db.run(CREATE_USERS_TABLE, (err) => {
  if (err) {
    return console.error(err.message);
  }
  console.log('Tabla usuarios creada');
});

// exportar para usar en otras partes
module.exports = db;
```

3. Ejecutar este archivo para crear la BD:

```
node database.js
```

4. Usar la BD en el registro con:

```js
const db = require('./database');

// registrar usuario
db.run('INSERT INTO users (name, email, password) VALUES (?, ?, ?)',
  [name, email, hashedPassword] 
);
```

5. Consultar la BD para la autenticación:

```js 
db.get('SELECT * FROM users WHERE email = ?', [email], (err, row) => {
  // verificar password
  // crear sesión
});
```

De esta forma tendremos una BD SQLite configurada para guardar los usuarios de la aplicación.
