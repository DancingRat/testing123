# **Set up of a project**
# Front-end
## For 純HTML,JavaScript, and CSS
- have to at least set up a public folder for guest; and a private folder for logged in people
  - the HTML file inside should connect to a js and a CSS file, with some libraries/framework, such as bootstrap, font-awesome, moment.js, google font etc.
  - login page like below
```html
  <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!--Google font-->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Luckiest+Guy&display=swap" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
    <link rel="stylesheet" href="./css/login.css" />
    <title>AI Music Lab</title>
</head>
<body>
<!-- Nav bar -->
<header class="">
    <section class="vh-100 bg-image">
        <div class="mask d-flex align-items-center h-100 gradient-custom-3">
            <div class="container h-100">
                <div class="row d-flex justify-content-center align-items-center h-100 margin-top-5">
                    <div class="col-12 col-md-9 col-lg-7 col-xl-6">
                        <div class="card" style="border-radius: 15px; margin-top: 20px;">
                            <div class="card-body p-5">
                                <h2 class="text-uppercase text-center mb-5">Login</h2>
                                <form id="form-login">
                                    <div class="form-outline mb-4">
                                        <input type="text" id="username" name="username" class="form-control form-control-lg"
                                            required />
                                        <label class="form-label" for="username">Your Username</label>
                                    </div>
                                    <div class="form-outline mb-4">
                                        <input type="password" id="password" name="password"
                                            class="form-control form-control-lg" required />
                                        <label class="form-label" for="password">Password</label>
                                    </div>
                                    <div class="form-check d-flex justify-content-center mb-5">
                                        <input class="form-check-input me-2" style="margin-top: 20px" type="checkbox"
                                            value="" id="checkbox" name="rememberMe" />
                                        <label class="form-check-label" style="margin-top: 16px" for="checkbox">
                                            Remember me<a href="#!" class="text-body"></a>
                                        </label>
                                    </div>
                                    <div class="d-flex justify-content-center">
                                        <button type="submit" class="btn btn-primary">Submit</button>
                                    </div>
                                    <p class="text-center text-muted mt-5 mb-0">Newcomer? <a href="./register.html"
                                            class="fw-bold text-body"><u>Register here</u></a></p>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p"
        crossorigin="anonymous"></script>
        <!--font awesome -->
        <script src="https://kit.fontawesome.com/58473b153d.js" crossorigin="anonymous"></script>
    <script src="./js/login.js"></script>
</body></html>
```
- For js example(login.js)
```javascript
window.onload = () => {
    initLoginForm();
  };
  function initLoginForm() {
    document.getElementById("form-login").addEventListener("submit", async function (e) {
      e.preventDefault();
      const form = e.target;
      const formObject = {};
      formObject.username = form.username.value;
      formObject.password = form.password.value;
      formObject.rememberMe = form.rememberMe.checked;
      console.log(formObject);
      const resp = await fetch("/api/users/login", {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(formObject),
      });
      if (resp.status === 200) {
        window.location.href = "index.html";
      } else {
        const data = await resp.json();
        alert(data.message);
      }
    });
  }
```
## For React
- Installations
  - for the main template
  ```
  npx create-react-app my-app --template typescript

  if you're using jsx rather than tsx, you can ignore the "--template typescript"
  ```
  - React routing
  ```
  yarn add react-router-dom@6 @types/react-router-dom react-router @types/react-router
  ```
  - React bootstrap(beside of react-bootstrap, you can still use the classic bootstrap)
  ```
  yarn add react-bootstrap@2
  yarn add bootstrap@5
  ```
  - Connecting React server to node server(or other server)
  ```
  yarn add cors @types/cors
  ```
  - React Hook Form(Optional)
  ```
  yarn add react-hook-form
  ```
  - Redux toolkit(Optional)
  ```
  yarn add react-redux redux @reduxjs/toolkit @types/react-redux
  ```
  - Testing library(Optional)
  ```
  yarn add @testing-library/react @testing-library/jest-dom
  ```
  - React Spinners(Optional)
  ```
  yarn add react-spinners
  ```
  - if you want a Mock/Virtual Server(Optional)
  ```
  yarn add json-server
  ```
- After installation, first thing we've to do in react is routing
  - Should open a 'page' folder inside the 'src' folder; the 'page' folder should include files for different pages(e.g. home page, login page, 404Not Found page etc.), which will then connect to the routes

- Index.tsx setup
```javascript
import React from "react";
import ReactDOM from "react-dom/client";
//css
import "bootstrap/dist/css/bootstrap.min.css";
import "./index.css";

import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { BrowserRouter } from "react-router-dom";
import { Provider } from "react-redux";
import store from "./redux/store";
const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  </React.StrictMode>
);
reportWebVitals();
```
# Back-end
## Installing yarn
- if you wanna use yarn rather than npm, you've to install with the following command
```
npm install -g yarn
```
- for the yarn command detail, you can check the following table

| npm  | yarn  |
|---|---|
|npm init   |yarn init   |
|npm install   |yarn install   |
|npm link   |yarn link   |
|npm install [package]   |yarn add [package]   |
|npm install [package] --save-dev   |yarn add --dev [package]   |
|npm install --global [package]   |yarn global add [package]   |
|npm run build   |yarn build   |
|npm start   |yarn start   |
|npm run dev   |yarn dev   |
|npm uninstall   |yarn remove   |

## For dotenv
- Installing dotenv
```
npm install grant  dotenv @types/dotenv
```
- create a .env file
    - Put all your database(DB) information like below, after created your database(such as psql db), including development & testing DB(if you have)
    ```
    NODE_ENV=development
    DB_NAME=<<database name>> 
    DB_USER=<<username of that database>>
    DB_PASSWORD=<<password of that database>>
    
    TEST_DB_NAME=<<Test database name>>    
    TEST_DB_USER=<<username of that database>>
    TEST_DB_PASSWORD=<<password of that database>>
    
    暫存
    POSTGRES_HOST=localhost 
    POSTGRES_DB=c19_bad_08_test  
    POSTGRES_USER=c19_bad_08  
    POSTGRES_PASSWORD=c19_bad_08
    暫存

    以上的名都是參考的名字(其實是網名),是可以改名的;但緊記要match返knexfile.ts or pgClient
    ```
- create a .env.example file
    - content same as .env file but shouldn't fill in the username and password of your DB(that may leak your DB info to anyone visit your project)
## For Typescript Project Setup
- install ts related packages

```bash
yarn add ts-node @types/node typescript
or
npm i ts-node @types/node typescript
```
- create and configure a file name as `tsconfig.json`
    - below is the configuration that we've to put into the file
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "lib": ["es6", "dom"],
    "sourceMap": true,
    "allowJs": true,
    "jsx": "react",
    "esModuleInterop": true,
    "moduleResolution": "node",
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "suppressImplicitAnyIndexErrors": true,
    "noUnusedLocals": true
  },
  "exclude": ["node_modules", "build", "scripts", "index.js"]
}
```
- create and configure a file name as `index.js`
    - below is the configuration that we've to put into the file
```js
require("ts-node/register");
require(/*"./<<the path that include your node server(the ts file that've app.listen(PORT,()=>{}); )>>"*/);
```
- To run js files in node, we do 'node .\ <u>js file/path</u>'; but in ts file we've multiple methods to run
```
1st.    npx ts-node .\<<ts file/path>>
2nd.    node .\index.js
P.S. the 2nd method is only work when 'index.js' is created and configured from above step
```
## Setting up .gitignore file
- create and configure a file name as `.gitignore`
    - below are the files that at least we've to include into the file; there're also some other files that we should include in .gitignore such as file include mass data, AI training stuff etc.
```text
node_modules
.DS_Store
.env
```
## Create hashPassword logic
1. install bcryptjs for hashing password
  ```
  yarn add bcryptjs @types/bcryptjs
  ```
2. create hash.ts(better put this file in folder named as utils), and include codes like below
```javascript
import * as bcrypt from "bcryptjs";
const SALT_ROUNDS = 10;
export async function hashPassword(plainPassword: string) {
  const hash = await bcrypt.hash(plainPassword, SALT_ROUNDS);
  return hash;
}
export async function checkPassword(plainPassword: string, hashPassword: string) {
  const match = await bcrypt.compare(plainPassword, hashPassword);
  return match;
}
```
## For Socket.io (optional)
- Installing
```
npm install socket.io
```
## For create and configure `.prettierrc` (optional)

```text
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": false,
    "singleQuote": true,
    "overrides": [
        {
            "files": ["*.ts", "*.js"],
            "options": {
                "semi": true,
                "tabWidth": 2,
                "singleQuote": false,
                "printWidth": 100
            }
        }
    ]
}

```
## MVC Setup for node/express server(in typescript)
- installing express related packages
```bash
npm i express @types/express
npm i ts-node-dev
npm i express-session @types/express-session

```
- installing other packages
    - bcryptjs for hashing password
    ```
    npm i bcryptjs @types/bcryptjs
    ```
    - jwt(with permit) for user authentication(alternative way of express session)
    ```
    yarn add jwt-simple @types/jwt-simple permit @types/permit
    ```
    - PG
    ```
    yarn add pg
    yarn add -D @types/pg
    ```
    - multer for form submission(if there's file upload/enclosed)
    ```
    1. npm install multer @types/multer
    2. you should open a folder call 'up', and a folder call 'upload' put inside 'up' folder; like the path below
    "./up/uploads"
    ```
- Server file(usually named as server.ts/main.ts)
  - below is the example
  ```javascript
  import dotenv from "dotenv";
  dotenv.config();
  import path from "path";
  import express from "express";
  import { Request, Response } from 'express';
  import expressSession from "express-session";
  import { Client } from "pg";
  import multer from "multer";
  import Knex from "knex";
  import type { Knex as KnexType } from "knex";
  //importing login checking logic from guards.ts
  import { isLoggedInStatic } from "./utils/guards";
  //DB config
  import * as knexConfigs from "./knexfile";
  const knex = Knex(knexConfigs[process.env.NODE_ENV || "development"]) as KnexType;

  let dbName;
  let dbUser;
  let dbPassword;
  switch (process.env.NODE_ENV) {
  case "development":
    dbName = process.env.DB_NAME;
    dbUser = process.env.DB_USER;
    dbPassword = process.env.DB_PASSWORD;
    break;
  case "test":
    dbName = process.env.POSTGRES_DB;
    dbUser = process.env.POSTGRES_USER;
    dbPassword = process.env.POSTGRES_PASSWORD;
    break;
  case "production":
    dbName = process.env.PROD_DB_NAME;
    dbUser = process.env.PROD_DB_USER;
    dbPassword = process.env.PROD_DB_PASSWORD;
    break;
  default:
    dbName = process.env.DB_NAME;
    dbUser = process.env.DB_USER;
    dbPassword = process.env.DB_PASSWORD;
  }
  export const dbClient = new Client({
  database: dbName,
  user: dbUser,
  password: dbPassword
  });
  dbClient.connect();

  //multer config
  const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, path.resolve("./up/uploads"));
  },
  filename: function (req, file, cb) {
    cb(null, `${file.fieldname}-${Date.now()}.${file.mimetype.split("/")[1]}`);
  },
  });
  export const upload = multer({ storage });

  // Create express instance
  const app = express();
  // Load express.json
  app.use(express.json());
  // Create express session
  app.use(
    expressSession({
      secret: "BAD08",
      resave: true,
      saveUninitialized: true,
    })
  );

  app.use((req, res, next) => {
    console.log(`[INFO] request path: ${req.path} method: ${req.method} ip: ${req.ip}`);
    console.log(req.session);
    next();
  });

  // Get into pages(in public folder) with path
  app.get('/', function (req: express.Request, res: express.Response) {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
  })
  app.get('/showcase', function (req: express.Request, res: express.Response) {
  res.sendFile(path.join(__dirname, 'public', 'showcase.html'));
  })
  // Get into pages(in private folder) with path
  app.get('/song_data', isLoggedInStatic, async function (req: Request, res: Response) {
  res.sendFile(path.join(__dirname, 'private', 'history.html'));
  })
  app.get('/profile', isLoggedInStatic, async function (req: Request, res: Response) {
  res.sendFile(path.join(__dirname, 'private', 'profile.html'));
  })

  // Route to check whether the current client to logged in
  app.get('/is_logged_in', async function (req: Request, res: Response) {
  if (req.session["user"]) {
    res.json({
      "message": "success",
      "id": req.session["user"]["id"]
    })
    return;
  }
  res.json({ "message": "not logged in" })
  })

  // Import controller and service(use user login page as an example )
  import { UserController } from "./controllers/UserController";
  import { UserService } from "./services/UserService";
  const userService = new UserService(knex);
  export const userController = new UserController(userService);

  // Import all routes from routes.ts(Routes的總部)
  import { routes } from "./routes";
  app.use("/api", routes)

  // Create static folders
  app.use(express.static(path.join(__dirname, "public")));
  app.use(isLoggedInStatic, express.static(path.join(__dirname, "private")));
  // Create static folders for multer storing files inside "./up/uploads"
  app.use(express.static(path.join(__dirname, "up")));

  // Listen to port 8080
  const PORT = 8080;
  app.listen(PORT, () => {
    console.log(`listening at http://localhost:${PORT}`);
  });
  ```
- User login check logic(isLoggedInStatic from guards.ts)
  - create a guards.ts and better put it in a folder named as utils, and include codes below
```javascript
import { Request, Response, NextFunction } from "express";
export function isLoggedInStatic(req: Request, res: Response, next: NextFunction) {
  if (req.session?.["user"]) {
    next();
  } else {
    res.redirect("/login.html");
  }
}
```
- create a routes.ts as a Routes的總部, including codes below
```javascript
import express from "express";
import { userRoutes } from "./routers/userRoutes";

export const routes = express.Router();

routes.use("/users", userRoutes);
```
- create a folder call 'routes', use for storing 分支routes, export to routes.ts eventually
  - e.g. I'm creating a file inside route folder, called userRoutes.ts; includes codes below
  ```javascript
  import express from "express";
  import { userController } from "../server";

  export const userRoutes = express.Router();

  userRoutes.post("/login", userController.login);
  ```
- create a folder call 'controllers', use for storing server logic, export to routes in route folder eventually
  - e.g. I'm creating a file inside controllers folder, called UserController.ts; includes codes below
  ```javascript
  import { UserService } from "../services/UserService";
  import { Request, Response } from "express";
  import { checkPassword } from "../utils/hash";
  export class UserController {
  constructor(private readonly userService: UserService) {}

  login = async (req: Request, res: Response) => {
    try {
      const { username, password } = req.body;
      if (!username || !password) {
        res.status(400).json({ message: "invalid username or password" });
        return;
      }
      const user = await this.userService.getUserByUsername(username);
      if (!user || !(await checkPassword(password, user.password))) {
        res.status(400).json({ message: "invalid username or password" });
        return;
      }
      req.session["user"] = { id: user.id };
      res.json({ message: "success" });
    } catch (err) {
      console.error(err.message);
      res.status(500).json({ message: "internal server error" });
    }
  };
  }
  ```
- create a folder call 'services', use for storing DB logic, export to controllers in controllers folder eventually
  - e.g. I'm creating a file inside services folder, called UserService.ts; includes codes below
  ```javascript
  import type { Knex } from "knex"; 
  export class UserService {
    constructor(private readonly knex: Knex) {}

  getUserByUsername = async (username: string) => {
    return this.knex("users")
      .select("id", "username", "password")
      .where({ username })
      .first();
  };
  }
  ```
## JWT(JSON Web Token) in node.js(MVC)
- The example above is authorizing user by req.session, but if we're using React, we usually use JWT instead
  - First, we've to create a config file call 'jwt.ts' and put it in utils
  ```javascript
  export default {
    jwtSecret: "Iamasecretthatyoushouldneverrevealtoanyone",
    jwtSession: {
        session: false
    }
  }
  ```
  - In guards.ts(in utils), instead of using logic with req.session, we'll do like below
  ```javascript
  import {Bearer} from 'permit';
  import jwtSimple from 'jwt-simple';
  import express from 'express';
  import jwt from './jwt';
  import { userService } from './server';
  //import { User } from './services/models';

  const permit = new Bearer({
    query:"access_token"
  })
  export async function isLoggedIn(
                        req:express.Request,
                        res:express.Response,
                        next:express.NextFunction){
    try{
        const token = permit.check(req);
        if(!token){
            return res.status(401).json({msg:"Permission Denied"});
        }
        const payload = jwtSimple.decode(token,jwt.jwtSecret);
        // Asking Database to get user info
        const user:User = await userService.getUserById(payload.id);
        if(user){
            req.user = user;
            return next();
        } else {
            return res.status(401).json({msg:"Permission Denied"});
        }
    }catch(e){
        return res.status(401).json({msg:"Permission Denied"});
    }
  }
  ```
  - Then we can import { isLoggedIn } to routes.ts or different routes in routes folder; and insert the 'isLoggedIn' into the route handlers like below
  ```javascript
  import express from "express";
  import { userController } from "../server";
  import { isLoggedIn } from './guards';

  export const userRoutes = express.Router();
  userRoutes.post("/login", userController.login);
  userRoutes.get('/boards',isLoggedIn,userController.list);
  userRoutes.post('/boards',isLoggedIn,userController.post);
  ```
  - In UserController
  ```javascript
  import {Request,Response} from 'express';
  import { UserService } from '../services/UserService';
  import jwtSimple from 'jwt-simple';
  import jwt from '../jwt';
  import {checkPassword} from '../hash';
  import fetch from 'node-fetch';

  export class UserController{
    constructor(private userService:UserService){}
    login = async (req:Request,res:Response)=>{
        try{
            if (!req.body.username || !req.body.password) {
                res.status(401).json({msg:"Wrong Username/Password"});
                return}
            const {username,password} = req.body;
            const user = (await this.userService.getUserByUsername(username))[0];
            if(!user || !(await checkPassword(password,user.password))){
                res.status(401).json({msg:"Wrong Username/Password"});
                return}
            const payload = {id: user.id, username: user.username};
            const token = jwtSimple.encode(payload, jwt.jwtSecret);
            res.json({token: token});
        }catch(e){
            console.log(e)
            res.status(500).json({msg:e.toString()})
        }
    }
  } 
  ```
  - In UserService
  ```javascript
  import type { Knex } from "knex";
  import { tables } from "../utils/db";
  //import { User } from "./models";

  export default class UserService {
  constructor(private knex: Knex) {}
  getUserById = (userId: number) => {
    return this.knex(tables.USER).select("id", "username", "password").where("id", userId).first();
  };
  getUserByUsername = (username: string) => {
    return this.knex(tables.USER)
      .select("id", "username", "password")
      .where("username", username)
      .first()};
  }
  ```
# Database
## For PSQL
- Setting up your database
    1. login your SQL Shell(the default DB name, username, and password should be 'postgres')
    2. create a database for your project by the SQL command below, and type '\c <u>newly created DB name</u>' to enter your newly created DB
        - P.S. you can list all your DB by typing '\l' in SQL shell
        - Moreover, you may also need to create a database for testing purposes
    ```sql
    CREATE DATABASE /*database name 可9改住先*/;
    ```
    3. better create a username and password for that DB, by running below SQL command
        - P.S. you can list all your user by typing '\du' in SQL shell
    ```sql
    CREATE USER /*Username 最好同DB名一樣*/ WITH PASSWORD '/*Password 最好同DB名一樣*/' SUPERUSER;
    ```
    4. beside of SQL shell, you can also connect your PSQL DB in VS code after installing the extension(SQLTools)
        - P.S. you can skip this step if you don't wanna connect in VS code, SQLTools is a optional tools
    ```
    1. press the 圓柱icon on the left side bar
    2. press 'Add new connection' and choose PostgreSQL
    3. In the connection Setting
        - Connection name 可9改
        - Connect using: Server and Port normally
        - Fill in the Database and Username
        - for the password, choose Save password then enter your password
        - can press 'TEST CONNECTION' then press 'SAVE CONNECTION'
    4. after filling in the setting, press 'CONNECT NOW'
    5. then a .sql file will be generated and you and type any SQL command inside, and right click on the code you typed in and press 'Run Selected Query' to run the command.
    ```
- Creating tables; you can seed data if you're using knex; but if you're just using PSQL, you've to run the CREATE TABLE command like below
```sql
CREATE TABLE /*table name*/(
    id SERIAL PRIMARY KEY,
    /*column name*/ /*data type*/ /*constrain e.g. not null, default something*/
);

E.g.
CREATE TABLE categories(
    id SERIAL PRIMARY KEY,
    category VARCHAR(255) NOT NULL
);

CREATE TABLE books(
    id SERIAL PRIMARY KEY,
    category_id INTEGER,
    name VARCHAR(255) NOT NULL,
    author VARCHAR(255) NOT NULL,
    publisher VARCHAR(255) NOT NULL,
    price DECIMAL NOT NULL,
    description TEXT NOT NULL,
    stock INTEGER NOT NULL,
    is_newreleased BOOLEAN NOT NULL,
    is_recommended BOOLEAN NOT NULL,
    is_shown BOOLEAN NOT NULL,
    image VARCHAR(255),
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
CREATE TABLE IF NOT EXISTS users(
    id SERIAL PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    password VARCHAR(255) NOT NULL,
    is_admin BOOLEAN NOT NULL DEFAULT false,
    remember_token VARCHAR,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
CREATE TABLE carts(
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL,
    book_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL DEFAULT 1,
    price DECIMAL NOT NULL,
    is_bought BOOLEAN NOT NULL DEFAULT false,
    is_cancelled BOOLEAN NOT NULL DEFAULT false,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (book_id) REFERENCES books(id)
);
```
        
## For Knex(on PSQL)
- Installing Knex
```
yarn add knex @types/knex pg @types/pg
```
- Create and configure Knex config file
    - create config file(knexfile.ts)
    ```
    for typescript,
    yarn knex init -x ts
    ```
    - then you should make it(knexfile.ts) like below
```javascript
import type { Knex } from "knex";
import dotenv from "dotenv";

dotenv.config();
const config: { [key: string]: Knex.Config } = {
    //以下development是connect去你的主DB
  development: {
    client: "postgresql",
    connection: {
      database: process.env.DB_NAME,
      user: process.env.DB_USER,
      password: process.env.DB_PASSWORD,
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: "knex_migrations"
    }
  },
    //以下test是connect去你的testing DB(for test case purposes)
  test: {
    client: "postgresql",
    connection: {
      host: process.env.POSTGRES_HOST,
      database: process.env.POSTGRES_DB,
      user: process.env.POSTGRES_USER,
      password: process.env.POSTGRES_PASSWORD,
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: "knex_migrations"
    }
  },

  staging: {
    client: "postgresql",
    connection: {
      database: "my_db",
      user: "username",
      password: "password"
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: "knex_migrations"
    }
  },
    //以下production是for你production & deployment
  production: {
    client: "postgresql",
    connection: {
      database: process.env.PROD_DB_NAME,
      user: process.env.PROD_DB_USER,
      password: process.env.PROD_DB_PASSWORD,
    },
    pool: {
      min: 2,
      max: 10
    },
    migrations: {
      tableName: "knex_migrations"
    }
  }
};
module.exports = config;
//以上.env之後的名都是參考的名字(其實是網名),是可以改名的
```
- Knex Migrations, for create & update table in SQL
    - create a migration file
    ```
    yarn knex migrate:make <<filename>>
    ```
    - type in Knex code/command, depends on your purpose(e.g. create table, update table); below is a reference
```javascript
//example of creating a table
import { Knex } from "knex";
export async function up(knex: Knex): Promise<void> {
    await knex.schema.createTable("users", (table) => {
        table.increments();
        table.string("username").notNullable().unique();
        table.string("password").notNullable();
        table.string("email");
        table.integer("user_level");
        table.timestamps(false, true);
      });
      await knex.schema.createTable("genders", (table) => {
        table.increments();
        table.string("gender");
      });
      await knex.schema.createTable("regions", (table) => {
        table.increments();
        table.string("region");
      });
      await knex.schema.createTable("instruments", (table) => {
        table.increments();
        table.string("instrument_name").notNullable().unique();
      });
      await knex.schema.createTable("models", (table) => {
        table.increments();
        table.string("model_name").notNullable().unique();
      });
      await knex.schema.createTable("seeds", (table) => {
        table.increments();
        table.string("seed_name");
        table.string("seed_path");
      });
      await knex.schema.createTable("recordings", (table) => {
        table.increments();
        table.integer("user_id").references("users.id");
        table.integer("instrument_id").notNullable().references("instruments.id");
        table.integer("model_id").notNullable().references("models.id");
        table.integer("seed_id").references("seeds.id");
        table.string("recording_name").notNullable();
        table.string("recording_path").notNullable();
        table.boolean("is_public").notNullable();
        table.boolean("is_hidden_by_admin");
        table.string("input_notes");
        table.timestamps(false, true);
      });
      await knex.schema.createTable("likes_dislikes", (table) => {
        table.increments();
        table.integer("user_id").references("users.id");
        table.integer("recording_id").references("recordings.id");
        table.boolean("is_liked")
        table.timestamps(false, true);
      });
}
export async function down(knex: Knex): Promise<void> {
    await knex.schema.dropTableIfExists("likes_dislikes");
    await knex.schema.dropTableIfExists("recordings");
    await knex.schema.dropTableIfExists("seeds");
    await knex.schema.dropTableIfExists("models");
    await knex.schema.dropTableIfExists("instruments");
    await knex.schema.dropTableIfExists("regions");
    await knex.schema.dropTableIfExists("genders");
    await knex.schema.dropTableIfExists("users");
}
//example of updating a table
export async function up(knex: Knex): Promise<void> {
    if (await knex.schema.hasTable("recordings")) {
        await knex.schema.table("recordings", (table) => {
            table.string("upload_file_recording_path");
        });
    }
}
export async function down(knex: Knex): Promise<void> {
    if (await knex.schema.hasTable("recordings")) {
        await knex.schema.table("recordings", (table) => {
            table.dropColumn("upload_file_recording_path");
        });
    }
}
//another example of updating a table
export async function up(knex: Knex) {
    if (await knex.schema.hasTable("genders")) {
        await knex.schema.alterTable("genders", (table) => {
            table.renameColumn("gender", "gender_name");
        });
    }
}
export async function down(knex: Knex) {
    if (await knex.schema.hasTable("genders")) {
        await knex.schema.alterTable("genders", (table) => {
            table.renameColumn("gender_name", "gender");
        });
    }
}
```
- then you can change your table version by migration
    - migrate to newer version
    ```
    yarn knex migrate:up
    ```
    - migrate to older version
    ```
    yarn knex migrate:down
    ```
    - migrate to latest version
    ```
    yarn knex migrate:latest
    ```

- Knex Seed, for seed/insert the initial data in table after Knex migration
    - create a seed file
    ```
    yarn knex seed:make <<filename>>
    ```
    - type in Knex code/command, for insert the initial data; below is a reference
    ```javascript
    import { Knex } from "knex";
    import { hashPassword } from "../utils/hash";
    export async function seed(knex: Knex): Promise<void> {
    // Deletes ALL existing entries
    await knex("recordings").del();
    await knex("instruments").del();
    await knex("users").del();
    // Inserts seed entries
    const user = { username: "alex", password: await hashPassword("1234") };
    await knex("users").insert(user);

    const demoInstrument = [{ instrument_name: "Acoustic Grand Piano"}, 
    {instrument_name: "Sitar"}, {instrument_name: "Banjo"}, {instrument_name: "Koto"}];
    let instrumentId = (await knex("instruments").insert(demoInstrument).returning("id"))[0]

    for(let i=1;i<10;i++){
    const demoRecording = {instrument_id: instrumentId.id, recording_name:`Demo Song${i}`};
    await knex("recordings").insert(demoRecording);
    }
    };
    ```