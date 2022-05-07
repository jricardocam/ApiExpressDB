Readme

 1.Como primer paso se agregan las dependencias

```
npm install express --save-dev
npm install prisma --save-dev
```

2. Se Inicializa prisma de la siguiente forma: `npx prisma ini`
3. Se Crea base de datos: `create database explorers_api;`
4. Se modifica el archivo `.env`
5. Se declara una nueva tabla en la DB en el archivo `schema.prisma`
6. Se versiona el nuevo modelo como nuevo cambio en nuestra base de datos, usando el comando: `npx prisma migrate dev --name init`.
7. Se crea un archivo `prisma/seed.js`
8. Se corre el archivo: `node prisma/seed.js`
9. Se verifica que se hayan agregado los explorers en terminal SQL `/select * from explorers;`

   ![img](image/Readme/1651892485361.png)
10. Se crea un Archivo `server.js`

    ```
    const express = require('express');
    const app = express();
    app.use(express.json());
    const port = process.env.PORT || 3000;

    // Require para usar Prisma
    const { PrismaClient } = require('@prisma/client');
    const prisma = new PrismaClient();

    app.get('/', (req, res) => {
      res.json({message: 'alive'});
    });

    app.listen(port, () => {
      console.log(`Listening to requests on port ${port}`);
    });
    ```
11. Se corre el server `node server.js`
12. Agrega un nuevo endpoint GET en tu `server.js` que regrese todos los explorers. Prúebalo en la url: `localhost:3000/explorers`

    ```
    app.get('/explorers', async (req, res) => {
      const allExplorers =  await prisma.explorer.findMany({});
      res.json(allExplorers);
    });
    ```
13. Agrega un nuevo endpoint GET que te regrese el explorer al enviar un ID por query params. Prúebalo en la url: `localhost:3000/explorers/1`

    ```
    app.get('/explorers/:id', async (req, res) => {
      const id = req.params.id;
      const explorer = await prisma.explorer.findUnique({where: {id: parseInt(id)}});
      res.json(explorer);
    });
    ```
14. Agrega un nuevo endpoint POST con el que vas a poder crear nuevos explorers.

    ```
    app.post('/explorers', async (req, res) => {
      const explorer = {
        name: req.body.name,
        username: req.body.username,
        mission: req.body.mission
       };
      const message = 'Explorer creado.';
      await prisma.explorer.create({data: explorer});
      return res.json({message});
    });
    ```
15. Agrega un nuevo endpoint PUT, en el cuál recibirás el ID del explorer a actualizar, y en el cuerpo del request los campos a actualizar, para este caso solo haremos el update del campo mission.

    ```
    app.put('/explorers/:id', async (req, res) => {
    	const id = parseInt(req.params.id);

    	await prisma.explorer.update({
    		where: {
    			id: id
    		},
    		data: {
    			mission: req.body.mission
    		}
    	})

    	return res.json({message: "Actualizado correctamente"});
    });
    ```
16.
