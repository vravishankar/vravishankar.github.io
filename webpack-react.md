# Build a React Application using Webpack 2
## 1. Prepare Project directory
```sh
projects>mkdir react-app
projects>cd react-app
projects>npm init -y
```
## 2. Install React
```sh
npm install --save react
npm install --save react-dom
```
## 3. Install Webpack
```sh
npm install --save-dev webpack
npm install --save-dev webpack-dev-server
```
## 4. Install HtmlWebpackPlugin
```sh
npm install --save-dev html-webpack-plugin
```
## 5. Install Loaders
```sh
npm install --save-dev css-loader
npm install --save-dev style-loader
npm install --save-dev json-loader
```
## 6. Install PostCSS and Normalize CSS
```sh
npm install --save-dev postcss-loader
npm install --save-dev precss
npm install --save-dev autoprefixer
npm install --save-dev normalize-css
npm install --save-dev postcss-easy-import
```
## 7. Babel
```sh
npm install --save-dev babel-core
npm install --save-dev babel-loader
npm install --save-dev babel-preset-es2015
npm install --save-dev babel-preset-react
npm install --save-dev babel-preset-react-hmre
```
## 8. Configure Babel .babelrc
```json
{
    "presets": ["react","es2015"],
    "env": {
        "development": {
            "presets": ["react-hmre"]
        }
    }
}
```
## 9. Configure webpack.config.js
```javascript
const webpack = require('webpack');
const path = require('path');

const HtmlWebpackPlugin = require('html-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');

const postcssimport = require('postcss-easy-import');
const precss = require('precss');
const autoprefixer = require('autoprefixer');

const HOST = process.env.HOST || 'localhost';
const PORT = process.env.PORT || 8080;

module.exports = {
    entry: {
        app: '/src/app.js'
    }
}
```









