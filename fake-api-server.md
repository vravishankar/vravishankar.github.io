# Create API Server With Fake Data Using json-server & faker 
## 1. Introduction
### json-server
json-server is a javascript library that helps to build an API server within minutes. Combining with Faker library, JSON-Server can be used to create and access loads of fake data that can be used for development and testing purpose.

First you need to create a json file and name it 'db.json'

For more on json-server please take a look at their official site in GitHub
### Faker - Javascript Library
Faker is a javascript library that can generate large number of fake data. Through its helper methods it can also create fake transactions or user interactions. It also supports multiple localities.

For more information please take a look at the official documentation site in Github [Faker](https://github.com/marak/Faker.js/) link

## 2. Installation
```sh
npm install -g json-server
```
_**db.json**_
```json
{
    "people": [
        {
            "id":0,
            "name": "John"
        }
    ]
}
```
```sh
json-server --watch db.json
```
Now if you go to http://localhost:3000/people you will get
```json
{
    "people": [
        {
            "id":0,
            "name": "John"
        }
    ]
}
```
Using tools like POSTMAN you can add/edit/update/remove data using appropriate HTTP methods like POST, PUT, DELETE & GET. Data will be added to memory but if you want to persist the data the server allows you take the snapshot of the data.

In the termial Hit 's' to create a snapshot of the db.json file.
## Generate Data
```sh
npm install faker
npm install lodash
```
_**generate.js**_
```javascript
module.exports = function() {
    var faker = require("faker");
    var _ = require("lodash");

    return {
        people: _.times(100,function(n) {
            return {
                id: n,
                name: faker.name.findName(),
                avatar: faker.internet.avatar()
            }
        })
    }
}
```
```sh
json-server generate.js
```
Go to browser and open http://localhost:3000 now you should have got 100 names and avatars in the json output.
