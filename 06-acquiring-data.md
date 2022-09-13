# 06 Acquiring data

## 2022-09-08

1. API GET calls
2. JSON
3. Curl/Bash
  - Using Curl to get JSON
  - Scripting Curl with a bash script
  - Bash STDOUT
  - Saving JSON to a file with Curl/Bash
4. Node
  - Reading JSON from a file in Node/JavaScript
  - Using Fetch to get JSON
  - Saving JSON to a file in Node
  - Holding JSON in memory
  - Displaying JSON on STDOUT

### Useful links

#### Web APIs

[public-apis: A collective list of free APIs for use in software and web development](https://github.com/public-apis/public-apis) 

[Open Meteo - URL Builder](https://open-meteo.com/en/docs#api_form)

#### JSON

[JSON Introduction](https://www.w3schools.com/js/js_json_intro.asp) - w3schools

[Introducing JavaScript Objects](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects) - MDN Web Docs

#### Curl and JSON

[Receiving JSON](https://everything.curl.dev/http/post/json#receiving-json) - Everything Curl

[Consuming Web API JSON Data Using curl and jq](https://thisdavej.com/consuming-web-api-json-data-using-curl-and-jq/) - Dave Johnson

[Parse JSON data using jq and curl from command line](https://medium.com/how-tos-for-coders/https-medium-com-how-tos-for-coders-parse-json-data-using-jq-and-curl-from-command-line-5aa8a05cd79b) - Parse JSON data using jq and curl from command line - Kaushal Shah

#### Node, Files, and JSON

[Reading and writing JSON files in Node.js: A complete tutorial](https://blog.logrocket.com/reading-writing-json-files-nodejs-complete-tutorial/) - Joseph Mawa, LogRocket Blog

[Reading and Writing JSON Files with Node.js](https://stackabuse.com/reading-and-writing-json-files-with-node-js/) - Scott Robinson, StackAbuse

[Read/Write JSON Files with Node.js](https://heynode.com/tutorial/readwrite-json-files-nodejs/) - hey(node)

[How to read and write JSON file using Node.js](https://www.geeksforgeeks.org/how-to-read-and-write-json-file-using-node-js/) - GeeksForGeeks

[How to work with Node.js and JSON file](https://www.geeksforgeeks.org/how-to-work-with-node-js-and-json-file/) - GeeksForGeeks

#### Node Fetch

[Node Fetch](https://www.npmjs.com/package/node-fetch) - npm

[node-fetch: A light-weight module that brings Fetch API to Node.js](https://github.com/node-fetch/node-fetch)

[Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) - MDN Web Docs

[The Fetch API is finally coming to Node.js](https://blog.logrocket.com/fetch-api-node-js/) - Elijah Asaolu, LogRocket Blog

### Notes

#### Weather data

We'll get some weather data from Open Meteo (see ULR builder above) using Curl:

```
curl "https://api.open-meteo.com/v1/forecast?latitude=35.92&longitude=-79.05&current_weather=true&temperature_unit=fahrenheit&windspeed_unit=mph&precipitation_unit=inch&timezone=America%2FNew_York"
```
The output is just a string of JSON, which is fine for a computer to read, but hard for us. If we want to be able to see it in a pretty format, we can use a tool called `jq`, which is a command line implementation of jQuery. 

```
curl "https://api.open-meteo.com/v1/forecast?latitude=35.92&longitude=-79.05&current_weather=true&temperature_unit=fahrenheit&windspeed_unit=mph&precipitation_unit=inch&timezone=America%2FNew_York" | jq
```

What we're seeing here is the current weather in data.

Another way that we can write this command is as follows: 

```
curl -s -G -d "latitude=35.92&longitude=-79.05" https://api.open-meteo.com/v1/forecast | jq
```

Which is shorthand for:

```
curl --silent --get --data 'latitude=35.92&longitude=-79.05' https://api.open-meteo.com/v1/forecast 
```

Let's save that to a file:

```
curl --silent --get --data 'latitude=35.92&longitude=-79.05' https://api.open-meteo.com/v1/forecast --output current_weather.json
```

Another way we can save to a file, if you will remember, is to redirect STDOUT into a file: 

```
curl --silent --get --data 'latitude=35.92&longitude=-79.05' https://api.open-meteo.com/v1/forecast > current_weather.json
```

Let's look at that file: 

```
cat current_weather.json
```

You'll notice that the command line hangs weirdly.
This is because there is not an end of line character at the end of the line we wrote into the file.

Most of the time this doesn't matter, but sometimes it does, so it is good practice to try to make your files have a line break at the end. One way is to add `printf "\n"` which will put a non-printing newline character at the end. 

This is a vestige of another era in which we might have been actually printing this output onto a physical medium. The old ways are always with us somewhere in there.

```
curl --silent --get --data 'latitude=35.92&longitude=-79.05' https://api.open-meteo.com/v1/forecast > current_weather.json && printf "\n" >> current_weather.json
```

Remember that when you want to overwrite a file, you use `>` but when you want to append more to a file you use `>>`.

#### Reading and writing files in Node

Okay, so we have some data in a file.

Let's read that in using Node.

First things first, let's set up a package so we can install things if we need to (hint, we will need to). 

```
npm init
```

And then let's write a script to read a file.

```weather.js
// Load built-in fs module using CommonJS syntax
const fs = require("fs");
// Read our JSON file into a variable
let currentJSON = fs.readFileSync("./current_weather.json"); 
// Put the data back out onto STDOUT
console.log(currentJSON);
```

What's happening here? 

Well, we are reading a JavaScript object into a variable from a file and then echoing that back onto STDOUT but it is not going to make any sense because all we are seeing is the memory buffer.
We need to convert the object BACK into a string in order to make it make sense for us.

```weather.js
// Load built-in fs module using CommonJS syntax
const fs = require("fs");
// Read our JSON file into a variable
let currentJSON = fs.readFileSync("./current_weather.json"); 
// Convert data back to string
let currentString = JSON.parse(currentJSON);
// Put the stringified data back out onto STDOUT
console.log(currentString);
```

#### Fetch

Fetch is part of the browser-side implementation of the V8 JS engine, but it will not be a built-in for Node until v18.x.x. For now we can install it using: 

```
npm install node-fetch
```

We're going to use Fetch to so the same thing that we just did with curl, but in JS. 

There is one thing though, node-fetch doesn't allow for us to load it using CommonJS (CJS) syntax. So, instead, we'll use the ECMAScript Method (ESM).

```fetch_weather.js
// Load fetch
import fetch from 'node-fetch';
// Nake a request
const response = await fetch('https://api.open-meteo.com/v1/forecast?latitude=35.92&longitude=-79.05&current_weather=true&temperature_unit=fahrenheit&windspeed_unit=mph&precipitation_unit=inch&timezone=America%2FNew_York');
// Get the data from the request
const data = await response.json();
// Log the data onto STDOUT
console.log(data);
```

And then let's run it with Node:

```
node fetch_weather.js
```

This, unfortunately, gives us this error:

```
(node:3003) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
(Use `node --trace-warnings ...` to show where the warning was created)
/home/john/Workspace/acquire2/weather.js:2
import fetch from 'node-fetch';
^^^^^^

SyntaxError: Cannot use import statement outside a module
    at Object.compileFunction (node:vm:360:18)
    at wrapSafe (node:internal/modules/cjs/loader:1055:15)
    at Module._compile (node:internal/modules/cjs/loader:1090:27)
    at Object.Module._extensions..js (node:internal/modules/cjs/loader:1180:10)
    at Module.load (node:internal/modules/cjs/loader:1004:32)
    at Function.Module._load (node:internal/modules/cjs/loader:839:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12)
    at node:internal/main/run_main_module:17:47
```

BUT, lucky for us, the error tells us _EXACTLY_ what to do to fix it and gives us two options.

Let's take the easier of the two and change the file extension.

```
mv fetch_weather.js fetch_weather.mjs
```

What happens when we run it again? 

```
node fetch_weather.mjs
```

_Et voila:_

```
{
  latitude: 35.875,
  longitude: -79,
  generationtime_ms: 0.30303001403808594,
  utc_offset_seconds: -14400,
  timezone: 'America/New_York',
  timezone_abbreviation: 'EDT',
  elevation: 127,
  current_weather: {
    temperature: 75.2,
    windspeed: 3.1,
    winddirection: 201,
    weathercode: 80,
    time: '2022-09-12T20:00'
  }
}
```

##### Write out to a file
