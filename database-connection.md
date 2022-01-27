# How to connect to a Database (NodeJS) and pass the data to React front end

## 1. Open PSQL in the command line (from project directory)

1. `CREATE DATABASE databasename;`

2. Connect to the db: `\c databasename`

## 3. In the project, set up database connection

install pg package `npm install pg`

in main node server file:

```
const { Pool } = require("pg")

let dotenvPath = ""; // specify path to .env file

let ORIGIN_URL = "";
if (process.env.NODE_ENV === "production") {
  console.log("running in production!");
  ORIGIN_URL = "*";
  dotenvPath = "../.env";
} else {
  console.log("running in development!");
  ORIGIN_URL = "*";
  dotenvPath = "../.env";
}

require("dotenv").config({ path: dotenvPath });


// Postgres database connection set up
const dbParams = require("./db/dbParams");

const db = new Pool(dbParams);
db.connect(() => console.log("✅ connected to db"));

// setup cors options
const corsOptions = {
  origin: ORIGIN_URL,
};
app.use(cors(corsOptions));

```

## 4. create `dbParams.js` in the `db` folder:

dbParams: 

```
require("dotenv").config();

let dbParams = {};

if (process.env.DATABASE_URL) {
  dbParams.connectionString = process.env.DATABASE_URL;
} else {
  dbParams = {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    user: process.env.DB_USER,
    password: process.env.DB_PASS,
    database: process.env.DB_NAME,
  };
}

module.exports = dbParams;

```


## 5. add required parameters to .env file

```
DB_HOST=localhost
DB_USER=postgres
DB_PASS=projectname
DB_NAME=projectname
DB_PORT=5432
```

## 6. Check node console to see if database connection is successful


## 7. In the project, create seeds.sql and schema.sql 

In `server/db` create these files and add the code for schema & seeds.

In the terminal, once connected to the correct database (from the correct project directory), run the schema file, and then the seeds file. 

`\i server/db/schema.sql`
`\i server/db/seeds.sql`


