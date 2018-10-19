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

make sure to add .gitignore before pushing

-----
