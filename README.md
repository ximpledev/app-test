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

2-1. using npm install (git+) with HTTPS

- node unlink lib-test

- add .gitignore
node_modules/

- go to lib-test GitHub repo, press 'Copy or download' button

- choose 'Clone with HTTPS', copy the URL
in this case:
https://github.com/ximpledev/lib-test.git

- modify package.json

"dependencies": {
  "lib-test": "git+",
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
app-test can automatically use it without help of any command.

-----

2-2. using npm install (git+) with SSH

- npm uninstall lib-test

- go to lib-test GitHub repo, press 'Copy or download' button

- choose 'Clone with SSH', copy the URL
in this case:
git@github.com:ximpledev/lib-test.git

- modify package.json

"dependencies": {
  "lib-test": "git+ssh://",
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

ps,
if lib-test has modifications,
- npm update
then app-test can use the latest lib-test.

-----
