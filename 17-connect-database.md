# 18 Database connection

## 2022-10-27

### Agenda

1. SQL v. NoSQL
2. SQLite
3. Set up a database connection using better-sqlite
4. Creating a log database
5. (Write a log file with Morgan)

### Useful links

#### Generating logs

[Express.js and Morgan Logging](https://www.loggly.com/use-cases/express-js-and-morgan-logging/)

[How to use morgan in your Express project](https://www.digitalocean.com/community/tutorials/nodejs-getting-started-morgan) - Cooper Makhijani

[Morgan NPM Logger - The Beginner's Guide](https://coralogix.com/blog/morgan-npm-logger-the-complete-guide/) - Fernando Doglio

[Morgan NPM Logger (source code)](https://github.com/expressjs/morgan)

[Morgan (npm package)](https://www.npmjs.com/package/morgan)

[A guide to Node.js logging](https://www.twilio.com/blog/guide-node-js-logging) - Dominik Kundel

[Node.js logging made easy: A tutorial on just about everything you need to know](https://sematext.com/blog/node-js-logging/) - Adnan Rahić

#### Writing log files

[Morgan](https://expressjs.com/en/resources/middleware/morgan.html) - Express

[Logging](https://riptutorial.com/express/topic/7191/logging) - RIP Tutorial

[Node.js logging - How to get started](https://www.papertrail.com/solution/tips/node-js-logging-how-to-get-started/)

[A complete guide to Node.js logging (with the best practices for logging)](https://betterprogramming.pub/a-complete-guide-to-node-js-logging-1ba70a4a346d) - Yasas Sandeepa

[How to get started with logging in Node.js](https://betterstack.com/community/guides/logging/how-to-start-logging-with-node-js/) - Better Stack Team


[Express Database Integration Guide](https://expressjs.com/en/guide/database-integration.html)

[Database integration in Express](https://www.geeksforgeeks.org/database-integration-in-express-js/)

#### SQLite3

[better-sqlite3 API documentation](https://github.com/JoshuaWise/better-sqlite3/blob/master/docs/api.md)

[better-sqlite3](https://www.npmjs.com/package/better-sqlite3) - NPM

[SQLite3 data types](https://www.sqlite.org/datatype3.html)

#### regex

[Regex tutorial](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285) - Jonny Fox

[regular expressions 101](https://regex101.com)

[Regex Crossword](https://regexcrossword.com/)

### Notes

In class on Tuesday, we discussed writing an access log to a file instead of just echoing to STDOUT.

We will use the `morgan` NPM package to do this.

`morgan()` takes two arguments: a log type and an object with options.

In our case, we want to [write the log output to a stream using the FS module](https://www.npmjs.com/package/morgan#write-logs-to-a-file).

The end result will be this: 

```server.js
// Use morgan for logging to files
// Create a write stream to append to an access.log file
const accessLog = fs.createWriteStream('access.log', { flags: 'a' })
// Set up the access logging middleware
app.use(morgan('combined', { stream: accessLog }))
```

The above code will produse a file `access.log` that looks like this: 

```access.log
::1 - - [22/Oct/2022:01:45:47 +0000] "GET /app/flips/60 HTTP/1.1" 200 523 "-" "curl/7.74.0"
::1 - - [22/Oct/2022:01:46:08 +0000] "GET /app/flip/call/heads HTTP/1.1" 200 47 "-" "curl/7.74.0"
::1 - - [22/Oct/2022:01:46:14 +0000] "GET /app/flips/700 HTTP/1.1" 200 5645 "-" "curl/7.74.0"
::1 - - [22/Oct/2022:01:46:23 +0000] "GET /app/flips/9000 HTTP/1.1" 200 72047 "-" "curl/7.74.0"
::1 - - [22/Oct/2022:01:46:32 +0000] "GET /app/flips/90000 HTTP/1.1" 200 720049 "-" "curl/7.74.0"
::1 - - [22/Oct/2022:22:57:18 +0000] "GET /app/flip/ HTTP/1.1" 200 16 "-" "curl/7.74.0"
```

If we want to create the same or similar type of log, but as a database table that we can then use to present log information in an interface, we have a few options.

We will go with the option that involves extracting specific information from the Express `req` object and placing that information in the appropriate field in a database table.

```database.js
// Require better-sqlite3
// const database = require('better-sqlite3')
// Import better-sqlite3
import { database } from "better-sqlite3"
// Create a database connection
const db = new database('rolldice.db')
// Check to see if access log table exists
const stmt = db.prepare(`SELECT name FROM sqlite_master WHERE type='table' and name='access';`)
let row = stmt.get();
// If access log table doesn't exist, create it.
if (row === undefined) {
    console.log('Log database appears to be empty. Creating log database...')
// 
    const sqlInit = `
        CREATE TABLE accesslog ( 
            id INTEGER PRIMARY KEY, 
            remote_addr VARCHAR, 
            remote_user VARCHAR, 
            date VARCHAR, 
            method VARCHAR, 
            url VARCHAR, 
            http_version VARCHAR, 
            status INTEGER, 
            content_length VARCHAR,
            referer_url VARCHAR,
            user_agent VARCHAR
        );
    `

    db.exec(sqlInit)
} else {
    console.log('Database exists.')
}

export default db;
```

The above script will check for an access log table in a database, and if it doesn't exist, it will initialize the DB and create a table that will hold all of the relevant pieces of information to be able to reproduce a combined log entry.
