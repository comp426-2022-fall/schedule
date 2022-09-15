# 07 Scripting

## 2022-09-15

1. Documentation
2. Planning
3. Structure
4. Environment

### Useful links

#### Node scripting

[How To Write Your First Node.js Script](https://devdojo.com/bo-iliev/how-to-write-your-first-nodejs-script) - @bo-iliev, DevDojo

[Writing a Node.js script](https://medium.com/@ian.mundy/writing-a-node-js-script-4baef96104da) - Ian Mundy

[Node.js: writing shell scripts using modern JavaScript instead of Bash](https://blog.cloudboost.io/node-js-writing-shell-scripts-using-modern-javascript-instead-of-bash-774e0859f965) - Vladimir Tolstikov

[Executing Shell Commands with Node.js](https://stackabuse.com/executing-shell-commands-with-node-js/) - Arpan Abhishek, StackAbuse

[Process](https://nodejs.org/api/process.html)

#### Building NPM packages

[How to document an NPM package](https://www.geeksforgeeks.org/how-to-document-npm-packages/)

[Building an NPM package](https://developers.deepgram.com/blog/2021/12/build-npm-packages/) - Kevin Lewis

[A guide to creating a Node.js command line package](https://x-team.com/blog/a-guide-to-creating-a-nodejs-command/) - Rubens Mariuzzo, X-Team

### Notes

#### Help message as documentation

```
Usage: galosh.js [options] -[n|s] LATITUDE -[e|w] LONGITUDE -z TIME_ZONE

    -h            Show this help message and exit.
    -n, -s        Latitude: N positive; S negative.
    -e, -w        Longitude: E positive; W negative.
    -z            Time zone: uses tz.guess() from moment-timezone by default.
    -d 0-6        Day to retrieve weather: 0 is today; defaults to 1.
    -j            Echo pretty JSON from open-meteo API and exit.
```

#### Comments as planning/structure

```
// Dependencied

// Default action

// Process options

// Core function

// Structure output

// Cleanup and exit (if necessary)

