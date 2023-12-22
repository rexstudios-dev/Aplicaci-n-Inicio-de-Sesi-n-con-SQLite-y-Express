

1. Instalar dependencias:

```
npm install express express-session bcryptjs
```

2. Configurar Express en index.js:

```js
// index.js

const express = require('express');
const session = require('express-session');
const app = express();

app.use(express.urlencoded({ extended: false }));
app.use(session({
  secret: 'mi-secreto',
  resave: false,
  saveUninitialized: false 
}));

// rutas aquí

app.listen(3000);
```

3. Rutas para registro y login:

```js
const bcrypt = require('bcryptjs');

app.post('/register', (req, res) => {

  let { name, email, password } = req.body;
  
  // validaciones

  let hashedPassword = bcrypt.hashSync(password, 10);

  db.run('INSERT INTO users (name, email, password) VALUES (?, ?, ?)', 
    [name, email, hashedPassword]
  );
  
  res.redirect('/login');

});

app.post('/login', (req, res) => {

  let { email, password } = req.body;

  db.get('SELECT * FROM users WHERE email = ?', [email], (err, row) => {
    
    if (bcrypt.compareSync(password, row.password)) {
      req.session.userId = row.id;
      res.redirect('/profile');
    } else {
      res.send('Credenciales inválidas');
    }

  });

});
```

4. Rutas protegidas como /profile verificarían la sesión antes de renderizar.

