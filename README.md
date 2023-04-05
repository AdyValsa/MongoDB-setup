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
router.get("/", getAllUsers)

module.exports = router;


### controllers/users.js content:

const getAllUsers = (req,res) => {
 try {
   return res.status(200).json({message : "All users here"})
 } catch (error) {
   return res.status(500).json({message : error.message}) 
 }
}

module.exports = { getAllUsers };





