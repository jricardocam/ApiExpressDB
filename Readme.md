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
