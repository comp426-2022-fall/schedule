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
