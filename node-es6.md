# Node Server in ES6 (Using Babel)
#### Install the Dependencies
```sh
$ npm install --save-dev babel-cli
```
```sh
$ npm install --save-dev babel-preset-es2015 babel-preset-stage-2
```
```sh
$ npm install express
```
#### Create Express Server
```javascript
import express from 'express';
const app = express();
const port = 3000;

app.listen(port, () => {
  console.log(`Server running in port ${port}`);
})
```
Update the following in package.json
```json
...

"scripts": {
  "start": "babel-node index.js --presets es2015,stage-2"
}

...
```
#### Start the Server.
```sh
$ npm start
```
#### Using Nodemon
```sh
$ npm install --save-dev nodemon
```
#### Update npm start
```json
"scripts": {
  "start": "nodemon index.js --exec babel-node --presets es2015,stage-2"
}
```
```sh
$ npm start
```

### Getting ready for production use

#### First lets move server index.js to lib/index.js
```sh
$ mv index.js lib/index.js
```
#### Update npm start script
#### Lets add two new tasks

```json
"scripts": {
  "start": "nodemon lib/index.js --exec babel-node --presets es2015,stage-2",
  "build": "babel lib -d dist --presets es2015,stage-2",
  "serve": "node dist/index.js"
}
```

#### Now we can use **npm run build** and **npm run serve**
```sh
$ npm run build
$ npm run serve
```

#### Lets not forget to add dist to our **.gitignore** file:
```sh
$ touch .gitignore
```
```sh
dist
```

#### Saving Babel options to .babelrc
```sh
touch .babelrc

{
  "presets": ["es2015", "stage-2"],
  "plugins": []
}
```

#### Now we can remove the duplicated options from our npm scripts
```sh
"scripts": {
  ...
  "serve": "node dist/index.js"
}
```

### Testing the Server
#### Lets install mocha.
```sh
$ npm install --save-dev mocha
```

#### create test/index.js
```sh
$ mkdir test
$ touch test/index.js
```

```javascript
import http from 'http';
import assert from 'assert';

import '../lib/index.js';

describe('Example Server',() => {
  it('should return 200', done => {
    http.get('http://localhost/', res => {
      assert.equal(200,res.statusCode);
      done();
     });
   });
});
```
#### Install babel-register for the require hook.
```sh
$ npm install --save-dev babel-register
```

```json
  "scripts": {
    "start": "nodemon lib/index.js --exec babel-node",
    "build": "babel lib -d dist",
    "serve": "node dist/index.js",
    "test": "mocha --compilers js:babel-register"
  }
```
#### Now run the tests
```sh
$ npm test
```

### Thats all folks.
