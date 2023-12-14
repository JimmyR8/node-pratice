# README.md

## Express-Postgres-API

Este repositorio contiene una API simple construida con Express.js e integrada con una base de datos PostgreSQL para realizar operaciones básicas de CRUD en una tabla de "productos". La API incluye validación de clave API para una mayor seguridad.

### Prerrequisitos

Antes de ejecutar la aplicación, asegúrate de tener instalados los siguientes requisitos:

- [Node.js](https://nodejs.org/)
- [PostgreSQL](https://www.postgresql.org/)

### Configuración

1. Clona el repositorio:

   ```bash
   git clone <URL_del_repositorio>
   ```
2. Instala las dependencias>

   ```bash
   npm install
   ```
3. Configura las variables de entorno:
   Crea un archivo `.env` en el directorio raiz y agrega las siguientes variables:

    ```env
    API_KEY=<tu_clave_api>
   POSTGRES_USER=<tu_usuario_postgres>
   POSTGRES_HOST=<tu_host_postgres>
   POSTGRES_PASSWORD=<tu_contraseña_postgres>
   POSTGRES_DATABASE=<tu_base_de_datos_postgres>
   ```

## Configuracion
La aplicación utiliza el framework [Express](https://expressjs.com/) para la API y [pg](https://node-postgres.com/) para la conectividad con la base de datos PostgreSQL. Los detalles de conexión a la base de datos se configuran en el objeto `pool`.

  ```javascript
  const pool = new Pool({
    user: process.env.POSTGRES_USER,
    host: process.env.POSTGRES_HOST,
    database: process.env.POSTGRES_DATABASE,
    password: process.env.POSTGRES_PASSWORD,
    port: 5432,
    ssl: { rejectUnauthorized: false }
});
```

## Validación de Clave API
La API incluye una función de middleware `apiKeyValidation` para validar las solicitudes entrantes según el encabezado `USER_API_KEY`. Asegúrate de incluir esta clave en los encabezados de la solicitud para una validación exitosa.

  ```javascript
  const apiKeyValidation = (req, res, next) => {
    const userApiKey = req.get('USER_API_KEY');
    if (userApiKey && userApiKey === API_KEY) {
        next();
    } else {
        res.status(401).send("Clave API inválida");
    }
}

app.use(apiKeyValidation);
```

## Ejecución de la Aplicación
Inicia la aplicación ejecutando el siguiente comando:

  ```bash
  npm start
  ```

La aplicación estará disponible en `http://localhost:701`. La consola mostrará "The App is ON" al inicio exitoso.

## Endpoints
- GET /products: Obtiene todos los productos de la base de datos.
- POST /products: Agrega un nuevo producto a la base de datos. Requiere `nameProduct`, `price` y `quantity` en el cuerpo de la solicitud.

  ## Ejemplo de Uso
  ```bash
  curl -X GET http://localhost:701/products
  ```
  ```bash
  curl -X POST -H "Content-Type: application/json" -H "USER_API_KEY: <tu_clave_api>" -d '{"nameProduct": "Nuevo Producto", "price": 19.99, "quantity": 10}' http://localhost:701/products
  ```

> [!IMPORTANT]
> Licencia
> Este proyecyo esta bajo la [Licencia MIT](https://es.wikipedia.org/wiki/Licencia_MIT).

# Sientete libre de modificar y utlizar este codigo segun tus requisitos de proyecto.
