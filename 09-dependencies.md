# 09 Dependency Management

## 2022-09-15

_Guest lecturer: Cam Simbeck_

1. Dependencies overview
2. Dependency injection
3. Libraries
4. Make your own!

### Useful links

#### Node dependencies

[Understanding dependency management with Node Modules](https://levelup.gitconnected.com/understanding-dependency-management-with-node-modules-1c47bcdee98b) - Jason Lai

[Node.js dependency management](https://developer.ibm.com/tutorials/learn-nodejs-manage-packages-in-your-project/) - J Steven Perry, IBM Developer

[Node.js – Dependency Management](https://dzone.com/articles/nodejs-dependency-management) - Jawad Hasan Shani, DZone

[It depends. The art of dependency management in Javascript](https://blog.softwaremill.com/it-depends-the-art-of-dependency-management-in-javascript-f1f9c3cde3f7) - Michał Ostruszka

#### package.json

[What is package.json?](https://heynode.com/tutorial/what-packagejson/) - hey(node)

[package.json: Specifics of npm's package.json handling](https://docs.npmjs.com/cli/v8/configuring-npm/package-json) - npm Docs

[Understanding package.json](https://javascript.plainenglish.io/understanding-package-json-33c810ea8acb) - Sanchitha SR

#### Dependency deprecation

[Your enterprise app is built on deprecated npm modules. 😱or 💅?](https://blog.tidelift.com/your-enterprise-app-is-built-on-deprecated-npm-modules) - Jeremy Katz

[How to handle npm WARN deprecated messages when installing dependencies](https://sebhastian.com/npm-warn-deprecated/) - Nathan Sebhastian

### Notes

#### What is a dependency

- Code that relies upon another piece of code to execute
- Usually a package of code that simplifies life

#### Examples of dependency code

##### Npm

```
npm init
```

This creates a new npm project.

```
npm install <dependency>
```

- Adds code package to your project
- Can be referenced at any point at the top of your files
  - ‘Using’ / ‘import’ / ‘#include’ / etc.

###### Express example

```
npm install express
```

Create new file, `<filename>.js` (replace `<filename>` with whatever name you like:

```
touch <filename>.js
```

Put this code in `<filename>.js`

```<filename>.js
const express = require('express')
const app = express()
 
app.get('/', function (req, res) {
    res.send('Hello World')
  })
 
  app.listen(3000)
```

Run it:

```
node <filename>.js
```

#### Dependency injection at runtime

- Copy and pastes code to top of file
- Invisible, unless viewed specifically by user in compiler

#### Dependency libraries

- Package.json file
- Trimming/adding dependencies for use

#### Creating your own dependencies

Create a new file `./modules/math.js`:

```
export function add(x, y) {
    return x + y
}
```
Create another new file `test.js`.

```
import {add} from "./modules/math.js"
 
console.log(add(1,2))
```

> Remember that in Node 16.x.x, you have to tell Node that this is an ESM package by adding `"type": "module"` to `package.json` (or renaming the file with the `.mjs` extension). Don't worry, though: the error messages will tell you precisely what you need to do.

#### The Good

- Don’t need to reinvent the wheel
- Reduces length of your code (kinda)
- Morally good copy-pasting of code

#### The Bad

- Circular dependencies
  - Clutters program with useless code, can propagate code, bad for testing, etc.
- Security issues
  - Coder negligence
  - Not know exactly what tools you are using
- Deprecation of packages
  - Based your entire code around “cool-guy-code” dependency? Sorry kid, its gone. Have fun finding another package that perfectly fits.

#### Take-aways

- Dependencies are just packages of code and functions
- Dependency management is key to flawless code
  - Delete unnecessary dependencies!
- Always install dependencies when pulling from a repo
  - Use `npm install`
  - This links your dependencies as modules in Node
- Always know what you are working with when developing code using dependencies
- Dependencies are omnipresent in coding. You will have dependencies to work with, but you can understand them better.
