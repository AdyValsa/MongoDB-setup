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

const express = require("express");
const cors = require("cors");
const morgan = require("morgan");
const bodyParser = require("body-parser");
const userRouter = require("./routes/users.js");

const app = express();

app.use(cors());
app.use(morgan("dev"));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended : true}));

app.use(userRouter);

const PORT = 3002;

app.get("/", (req,res) => {
  res.status(200).json({message: "server is running"});
});

app.listen(PORT , () => {
  console.log(`Server is running at http://localhost${PORT}`);
});





### routes/users.js content:

const router = require("express").Router();

router.get("/", getAllUsers)
router.get("/", getAllUsers)

module.exports = router;






