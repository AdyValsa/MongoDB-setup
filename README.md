# MongoDB-setup

Example with one route: /users

## Express server:

 - Create index.js 
 - npm init -y
 - create src folder, move index.js in that folder
 - npm install express
 - npm install cors
 - npm install --save-dev nodemon morgan dotenv
 - npm install body-parser

In package.json in "scripts":{....} add:
  "start" : "nodemon src/index.js",

### Folder structure:

- ApplicationName
   - src
      - models
      - views
      - controllers
      - routes
      

### src/index.js content:

const express = require("express");<br>
const cors = require("cors");<br>
const morgan = require("morgan");<br>
const bodyParser = require("body-parser");<br>
const userRouter = require("./routes/users.js");<br>

const app = express();

app.use(cors());<br>
app.use(morgan("dev"));<br>
app.use(bodyParser.json());<br>
app.use(bodyParser.urlencoded({extended : true}));

app.use(userRouter);

const PORT = 3002;

app.get("/", (req,res) => {<br>
  res.status(200).json({message: "server is running"});<br>
});

app.listen(PORT , () => {<br>
  console.log(`Server is running at http://localhost${PORT}`);<br>
});





### routes/users.js content:

const router = require("express").Router();

router.get("/", getAllUsers)<br>
router.get("/:id", getOneUser)

module.exports = router;


### controllers/users.js content:

const getAllUsers = (req,res) => {
 try {
   return res.status(200).json({message : "All users here"})
 } catch (error) {
   return res.status(500).json({message : error.message}) 
 }
}
<br>
const getOneUser = (req, res) => {
    try{
      const id= Number(req.params.id)
      const oneUser = users.find(user => user.id === id)
      if(!oneUser){
        return res.status(404).json({message:'This user does not exist'})}
        res.json({message: 'User info', oneUser})
    }catch(e){
      res.status(500).json({
        message: e.message,
      })
    }
  }

module.exports = { getAllUsers, getOneUser };


### Documentation:
- Install packages: [Swagger js-doc](https://www.npmjs.com/package/swagger-jsdoc) and [Swagger UI Express](https://www.npmjs.com/package/swagger-ui-express)
- App.js:
const swaggerUi = require('swagger-ui-express');
const swaggerJsdoc = require('swagger-jsdoc');
<br>
const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Users API',
      version: '1.0.0',
    },
  },
  apis: ['./controllers/*.js'], 
};
const openapiSpecification = swaggerJsdoc(options);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(openapiSpecification));


- Controllers.js:
1- Create a Schema:
/**
 *@swagger
 *components:
 *  schemas:
 *    User:
 *      type: object
 *      required:
 *        - name
 *        - email
 *        - id
 *      properties:
 *        id:
 *          type: number
 *          description: type a number for the id
 *        name:
 *          type: string
 *          description: name of the user
 *        email:
 *          type: string
 *          description: email of the user
 *      example:
 *          id: 1
 *          name: Maria
 *          email: maria@gmail.com
 */
 
 2. Create a tag:
  /**
 * @swagger
 * tags:
 *   name: Users
 */
 
 3. Create the documentation for each function/route:
 - getAllUsers:
  /**
 * @swagger
 * /users:
 *  get:
 *    summary: Returns all the users
 *    tags: [Users]
 *    responses:
 *      200:
 *        description: all the users
 *        content:
 *          application/json:
 *            schema:
 *              type: array
 *              items:
 *                $ref: '#/components/schemas/User'
 *      500:
 *        description: error
 */
 
 <br>
 - getOneUser:
 /**
 * @swagger
 * /users/{id}:
 *  get:
 *    summary: get the user with this id
 *    tags: [Users]
 *    parameters:
 *      - in : path
 *        name : id
 *        schema:
 *          type: number
 *        required: true
 *        description: user id
 *    responses:
 *      200:
 *        description: the user with this id
 *        content:
 *          application/json:
 *            schema:
 *                $ref: '#/components/schemas/User'
 *      404:
 *        description: the user with this id was not found
 *      500:
 *        description: server error
 */
