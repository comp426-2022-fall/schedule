# 09 Dependency Management

## 2022-09-22

1. Creating data
2. Formatting JSON
3. Function
4. Inputs and outputs

### Useful links

#### History

[JavaScript History](https://www.w3schools.com/js/js_history.asp) - w3schools

[Difference between ES5 and ES6](https://www.geeksforgeeks.org/difference-between-es5-and-es6/) - GeeksforGeeks

[Difference between ES6 and TypeScript](https://www.geeksforgeeks.org/difference-between-es5-and-es6/)

[Difference between JavaScript and TypeScript](https://www.geeksforgeeks.org/difference-between-typescript-and-javascript/)

#### Functions

[Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) - MDN Web Docs

[JavaScript Functions](https://www.w3schools.com/js/js_functions.asp)

[Javascript Intro to Functions](https://learn.co/lessons/javascript-intro-to-functions) - Learn.co

[JavaScript Functions](https://www.javascripttutorial.net/javascript-function/) - JavaScript Tutorial

[Functions](https://www.learn-js.org/en/Functions) - learn-js.org

[JavaScript Function Definitions](https://www.w3schools.com/js/js_function_definition.asp)

#### ESM arrow functions

[Javascript Arrow Function](https://www.w3schools.com/js/js_arrow_function.asp)

[Arrow functions for beginners](https://codeburst.io/javascript-arrow-functions-for-beginners-926947fc0cdc) - Brandon Morelli

[Arrow functions vs regular functions in JavaScript](https://levelup.gitconnected.com/arrow-function-vs-regular-function-in-javascript-b6337fb87032) - Madhavan Nagarajan

[When (and why) you should use ES6 arrow functions &mdash; and when you shouldn't](https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/) - Cynthia Lee

### Notes

#### History

JavaScript was invented in 1995 by Brendan Eich.

In 1996, it was developed into a standard scripting langage by ECMA's (then the European Computer Manufacturers Association) Technical Committe (TC39) and released as ECMAScript 1 (ES1) in 1997.

TypeScript was developed and released by MicroSoft in 2012 to deal with some of the deficits of JS as a programming language.

It is technically a superset of ES6 (which is a superset of ES5). 

We'll refer to anything related to ECMAScript as "ESM."

The current release of TypeScript (TS4.5) incorporates ES2022 modules and is supported by Node.js 16.x.x.

#### Semicolons

It turns out that the seemingly arbitrary and undisciplined way I use semicolons is... fine I guess?

I have always just used them intuitively based on what I thought was happening and I realize now that my usage was a vestige of writing bash scripts, where they have somewhat more explicit use as a command terminator. 
So when I would write one-liners, this was the mechanism that I used to put the lines together into one. 

Apparently some people get *mad* about semicolons in JS. 

This is a good discussion of what semicolons are doing in JS and where the parser inserts them automatically and why: https://www.freecodecamp.org/news/lets-talk-about-semicolons-in-javascript-f1fe08ab4e53/

So, my advice is to use them if you feel like it and if you ever encounter someone who gets super upset about them, then avoid that person.
