- npm init
version: (1.0.0) 0.0.1
license: (ISC) MIT

- add README.md

-----

1. using 'npm link'

[ref]
https://www.youtube.com/watch?v=N55jHr9qzpg

- add app.js

- add content to index.js with CommonJS style

const greet = require('lib-test');
console.log(greet('Jerry'));

- npm link lib-test

- node app
(or node app.js)

done.

ps,
if lib-test has modifications,
app-test can automatically use it without help of any command.

-----

2-1. using npm install (git+) with HTTPS (which is recommended by GitHub)

- node unlink lib-test

- add .gitignore
node_modules/

- go to lib-test GitHub repo, press 'Copy or download' button

- choose 'Clone with HTTPS', copy the URL
in this case:
'https://github.com/ximpledev/lib-test.git'

- modify package.json

"dependencies": {
  "lib-test": "git+(...)",
}

paste 'https://github.com/ximpledev/lib-test.git' after 'git+'
=>
"dependencies": {
  "lib-test": "git+https://github.com/ximpledev/lib-test.git"
}

- npm i

(there could be a GitHub login dialog pop-op,
fill in username & password, and we're good to go)

- node app

done.

ps,
if lib-test has modifications,
- npm update
then app-test can use the latest lib-test.

-----

2-2. using npm install (git+) with SSH

- npm uninstall lib-test

- go to lib-test GitHub repo, press 'Copy or download' button

- choose 'Clone with SSH', copy the URL
in this case:
'git@github.com:ximpledev/lib-test.git'

- modify package.json

"dependencies": {
  "lib-test": "git+ssh://(...)",
}

paste 'git@github.com:ximpledev/lib-test.git' after 'git+ssh://'
=>
"dependencies": {
  "lib-test": "git+ssh://git@github.com:ximpledev/lib-test.git"
}

- if we haven't set up SSH, follow the steps below to set up
[ref]
https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

- npm i

- node app

done.

after testing how to use npm install (git+),
uninstall it and, for convenience, use 'npm link' instead, if you wish

-----

3. using Babel

- npm i -D babel-cli babel-core babel-preset-env

- move app.js to src/app.js
(cuz we want to use src/ to put ES6 code and dist/ to put Babel-transpiled code)

- modify package.json
"scripts": {
  "build": "babel src/ -d dist/",
  ...
}

- npm run build
dist/ will be created but app.js in it hasn't been transpiled

- modify package.json
"scripts": {
  "prebuild": "rm -rf dist/",
  ...
}
to remove dist/ every time before building

- add .babelrc
{
  "presets": [
    "env"
  ]
}

- npm run build
now dist/app.js is transpiled

- we don't have to modify package.json
to let npm know what the primary main entry point is

"main": "app.js",
=>
"main": "dist/app.js",

cuz it's not a lib

- instead, we modify package.json
"scripts": {
  "start": "node dist/app",
  ...
}

- npm start

-----

4. Webpack alias

when using Webpack, you can use its 'alias' to replace 'npm link', if you wish

e.g.,
in webpack.config.js

add
const LIB_DIR = path.resolve(__dirname, '../libs/lib-test');
...
module.exports = (env={}) => {
  ...
  return {
    ...
    resolve: {
      extensions: ['.js', '.jsx'], // default: ['.js', '.json']
        alias: {
          styles: path.resolve(SRC_DIR, 'styles'),
          images: path.resolve(SRC_DIR, 'assets/images'),
          'lib-test': LIB_DIR
        }
      }
    },
    ...
  }
}

then you can use lib-test by importing it

e.g.,
import greet from 'lib-test';
...
const greeting = greet('Jerry');

-----

in my point of view,
'npm link' uses global folders,
that is, we can't use 'npm link' for two modules with the same name

'Webpack aliases' use relative paths

when using Webpack, Webpack alias is a better choice
but whether using Webpack or not, npm link is simpler

-----

p.s.,
We can see the global symlink that was created by running:
> ls -al $(npm root -g)

and the local symlink that was created by running:
> ls -al ./node_modules
(which is used by app-test)

-----

if app-test wants to unlink from lib-test,
lib-test can't call 'unlink' before app-test call 'unlink lib-test'
that is, we have to follow the order below...

1. lib-test calls > npm link
2. app-test calls > npm link lib-test
3. app-test calls > npm unlink lib-test
4. lib-test calls > npm unlink

the order below won't work...
1. lib-test calls > npm link
2. app-test calls > npm link lib-test
3. lib-test calls > npm unlink
4. app-test calls > npm unlink lib-test

be careful!
