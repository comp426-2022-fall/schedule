# 07 Manipulating data

## 2022-09-13

1. Data ingest
2. Reshaping data
3. Presenting data

### Useful links

#### Web APIs

[public-apis: A collective list of free APIs for use in software and web development](https://github.com/public-apis/public-apis) 

[Open Meteo - URL Builder](https://open-meteo.com/en/docs#api_form)

#### Node scripting

[How To Write Your First Node.js Script](https://devdojo.com/bo-iliev/how-to-write-your-first-nodejs-script) - @bo-iliev, DevDojo

[Writing a Node.js script](https://medium.com/@ian.mundy/writing-a-node-js-script-4baef96104da) - Ian Mundy

[A guide to creating a Node.js command line package](https://x-team.com/blog/a-guide-to-creating-a-nodejs-command/) - Rubens Mariuzzo, X-Team

[Node.js: writing shell scripts using modern JavaScript instead of Bash](https://blog.cloudboost.io/node-js-writing-shell-scripts-using-modern-javascript-instead-of-bash-774e0859f965) - Vladimir Tolstikov

[Executing Shell Commands with Node.js](https://stackabuse.com/executing-shell-commands-with-node-js/) - Arpan Abhishek, StackAbuse

#### jQuery

We will not necessarily use jQuery for extracting and manipulating data all the time, but it is something that you should have in your toolbox.
It is particularly useful for browser-side programming, but it can also be useful in Node/server-side programming involving ingest and reshaping of data.

[jquery](https://www.npmjs.com/package/jquery) - NPM

[How to use jQuery with Node.js](https://www.geeksforgeeks.org/how-to-use-jquery-with-node-js/) - GeeksForGeeks

[How to use jQuery on Node](https://medium.com/fbdevclagos/how-to-use-jquery-on-node-df731bd6abc7) - Oyetoke Tobi Emmanuel

[jQuery: write less, do more](https://jquery.com/)

### Notes

Your next assignment will be to build a command line weather app that ingests data from the Open-Meteo API and displays it in a variety of ways based on command line arguments.

In this session we will look over a Bash script model for what you will do and work through addresssing some of the necessary components that will allow you to replicate the existing script with Node instead of Bash.

To get us started, we'll look at a code sample Bash script called _galo.sh_ written by someone you know: https://github.com/jdmar3/galo.sh

#### Code from class demo

This is a demo of how to use variables to manipulate a string that we are feeding to `fetch()` as an argument:

```
// Load fetch
import fetch from 'node-fetch';

// Default action

// Process options


// Declare latitude
let latitude = '35.90'
// Declare longitude
let longitude = '-79.05'
// Core function
// Make a request
const response = await fetch('https://api.open-meteo.com/v1/forecast?latitude=' + latitude + '&longitude=' + longitude + '&hourly=temperature_2m,relativehumidity_2m,precipitation,surface_pressure&current_weather=true&temperature_unit=fahrenheit&windspeed_unit=mph&precipitation_unit=inch&timezone=America%2FNew_York&past_days=7');
// Get the data from the request
const data = await response.json();
// Structure output

// Cleanup and exit (if necessary)
// Log the data onto STDOUT
console.log(data);
```

Refer to the last session notes for the structure you see outlined in the comments above.

To use command-line arguments as variables, we have already used minimist, so we can do that again.

To load minimist and see what arguments look like, do this: 

```
// Dependencies
import minimist from 'minimist';
// Use minimist to parse command line arguments
const args = minimist(process.argv.slice(2))
// See what is stored in `args` by minimist
console.log(args)
```

Finally if we want to be able to return a help message when we run our script with `-h`, we can do the following:

```
// Dependencies
import minimist from 'minimist';

const args = minimist(process.argv.slice(2))
console.log(args)
// Default action
// Was the command called with `-h`?
if ( args.h ) {
// If yes, then log the help message onto STDOUT
console.log(`Usage: galosh.js [options] -[n|s] LATITUDE -[e|w] LONGITUDE -z TIME_ZONE

    -h            Show this help message and exit.
    -n, -s        Latitude: N positive; S negative.
    -e, -w        Longitude: E positive; W negative.
    -z            Time zone: uses tz.guess() from moment-timezone by default.
    -d 0-6        Day to retrieve weather: 0 is today; defaults to 1.
    -j            Echo pretty JSON from open-meteo API and exit.
`)
// And exit
process.exit(0)
}
```
