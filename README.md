# swagger-docs

How to add Swagger documentation for a simple `GET /api/user` endpoint. Here’s how you can set it up in your Express application:

### 1. Install Swagger Dependencies
If you haven't already, install the necessary Swagger dependencies:
```bash
npm install swagger-ui-express swagger-jsdoc
```

### 2. Set Up Swagger Configuration
Create a `swagger.js` file to define the Swagger configuration:
```js
const swaggerJsDoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Simple App API',
      version: '1.0.0',
      description: 'API documentation for the Simple App',
    },
    servers: [
      {
        url: 'http://localhost:5000', // Replace with your server URL
      },
    ],
  },
  apis: ['./routes/*.js'], // Path to the API docs
};

const specs = swaggerJsDoc(options);

module.exports = (app) => {
  app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(specs));
};
```

### 3. Create Route and Add Swagger Annotations
Create a route file `routes/user.js` and add Swagger annotations:
```js
const express = require('express');
const router = express.Router();

/**
 * @swagger
 * components:
 *   schemas:
 *     User:
 *       type: object
 *       properties:
 *         id:
 *           type: string
 *           description: The user ID
 *         name:
 *           type: string
 *           description: The user's name
 *       example:
 *         id: 12345
 *         name: John Doe
 */

/**
 * @swagger
 * /api/user:
 *   get:
 *     summary: Get user information
 *     tags: [User]
 *     responses:
 *       200:
 *         description: A single user
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/User'
 */
router.get('/user', (req, res) => {
  res.json({ id: '12345', name: 'John Doe' });
});

module.exports = router;
```

### 4. Integrate Swagger with Express
Modify your main server file (e.g., `app.js`) to include the Swagger setup and the route:
```js
const express = require('express');
const swaggerSetup = require('./swagger'); // Import Swagger setup
const userRoutes = require('./routes/user');

const app = express();

app.use(express.json());

// Swagger setup
swaggerSetup(app);

// Routes
app.use('/api', userRoutes);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

### 5. Access Swagger UI
Start your server and navigate to `http://localhost:5000/api-docs` to access the Swagger UI. You should see the interactive API documentation where you can test the `GET /api/user` endpoint.

With this setup, the Swagger UI will provide a user-friendly interface for testing the `GET /api/user` endpoint, showing the expected response format and allowing you to execute the endpoint directly from the browser.
