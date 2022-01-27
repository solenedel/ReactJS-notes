# How to connect to a Database (NodeJS) and pass the data to React front end

# PART 1: setting up the database in Postgres and Node 

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
DB_PASS=nameofdatabase
DB_NAME=nameofdatabase
DB_PORT=5432
```

## 6. Check node console to see if database connection is successful


## 7. In the project, create seeds.sql and schema.sql 

In `server/db` create these files and add the code for schema & seeds.

In the terminal, once connected to the correct database (from the correct project directory), run the schema file, and then the seeds file. 

`\i server/db/schema.sql`
`\i server/db/seeds.sql`

## 8. Make a test request from node to the database

```
app.get("/testdb", (req, res) => {
  const queryText = `SELECT * FROM users;`;

  db.query(queryText)
    .then((results) => {
      res.json(results.rows);
    })
    .catch((err) => {
      console.log(err);
      res.json([]);
    });
});
```


# PART 2: Sending data from database to React front end

In the previous code snippet, we are making a request from the node server to the database when we go to `http://localhost:8081/dbtest` and returning data from the database query. 

Now, we need a corresponding GET request made on the React front end using axios:

```
useEffect(() => {
    axios
      .get("http://localhost:8081/dbtest")
      .then((response) => {
        console.log("DATA FROM DATABASE", response.data);
      })
      .catch((err) => console.error(err));
  }, []);
```

By making the request on the front end to the same URL, `response.data` gives us the data we got from the database in the back end (`results.rows`);



