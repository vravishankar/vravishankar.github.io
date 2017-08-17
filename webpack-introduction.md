# An Introduction to Webpack
In modern front end development a developer need to bundle multiple assets and libraries like images, stylesheets, vendor libraries and not to mention their own application code. In a larger project, managing and updating these assets not only consumes more time but also make it difficult for day to day maintenance. Also the javascript development ecosystem is constantly changing with more and more libraries, transpilers (transforming from one source to another source) being added more frequently now. Developers also need to optimise and minimise the code that can provide better performance during retrieving, rendering and processing by the browser. All these needs to handled by the developer while building and deploying the application for production use. So how to handle this complex workflow process of bundling and maintaining the assets more efficiently and precisely. Enter **Webpack**.

**Webpack** is a modern application module bundler that helps the developer to manage the assets and libraries in a more efficient way by identifying the dependencies based on the application / module needs and packaging them in bundles or chunks for optimal loading by the browser. Webpack enforces modularization of the code base making it more easier for maintenance and also in the process helps to improve the overall performance of browser rendering.

Webpack is an extensible tool set that comes with set of loaders and plugins that can be used by the developers for specific needs like transpiling, stylesheets processing, source code linting and mimising or uglifying the code base.

## 1. Installation
>Webpack needs node and npm libraries for installation. So make sure your environment has the most stable version of >node and npm installed as a pre-requisite for Webpack installation.

#### Global Installation
_Webpack like any npm library can be installed either locally or globally. If you are going to use webpack for multiple projects it is recommended to install it globally._
For Global installation follow the steps given below:
```
npm init -y
npm install -g webpack
```
#### Local Installation
If you want to install webpack local to the project ensure you are adding it to development dependency only.
```
npm init -y
npm install --save-dev webpack
```
#### Command Line Invocation
To invoke webpack without configuration file
```sh
./node_modules/.bin/webpack src/index.js dist/bundle.js
```
or if you have installed globally
```sh
webpack src/index.js dist/bundle.js
```
## 2. Getting Started
#### Minimal Configuration
We can start webpack configuration with these minimal configs. Broadly webpack configurations are made of these four components.
* Entry - Entry point for your application
* Output - Directory & filename to which webpack will write the bundled file
* Loaders - Transformations that needs to be applied over the source code
* Plugins - To customise the webpack build process

_webpack.config.js_
```javascript
const path = require('path')
const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    }
}
module.exports = config
```
_index.html_
```html
<html>
    <head>
        <title>Webpack Introduction</title>
    </head>
    <body>
        <script src="/dist/bundle.js"></script>
    </body>
</html>
```
_index.js_
```javascript
console.log('Hello Webpack')
```
## 3. CSS Loader
CSS Loader is used to generate CSS files from the resolved imports of CSS file.
Install the css-loader to development dependency
```sh
npm install --save-dev css-loader
```
_main.css_
```css
body {
    background: red
}
```
_main.js_
```javascript
require('./main.css')
```
_index.html_
```html
<html>
    <head>
        <title>Webpack Introduction</title>
    </head>
    <body>
        <script src="/dist/bundle.js"></script>
    </body>
</html>
```
_webpack.config.js_
```javascript
const path = require('path')
const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: 'css-loader'
            }
        ]
    }
}
module.exports = config
```
>Note: Using CSS Loader alone is not enough to change the style of the document. We need to use another loader called style-loader that will create a style element within the html document

To install style-loader download it from NPM library and add it to the webpack.config.js file.
```sh
npm install --save-dev style-loader
```
_webpack.config.js_
```javascript
const path = require('path')
const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            }
        ]
    }
}
module.exports = config
```
**_output_**
```html
<html>
    <head>
        <title>Webpack Introduction</title>
        <style type="text/css">body {
            background: red;
        }</style>
    </head>
    <body>
        <script src="/dist/bundle.js"></script>
    </body>
</html>
```
## 4. Using SASS Loader
Download the sass-loader and sass node library from NPM
```sh
npm install --save-dev sass-loader node-sass
```
_main.js_
```javascript
require('./main.scss')
```
_main.scss_
```scss
$primary: red;
body {
    background: $primary;
}
```
_webpack.config.js_
```javascript
const path = require('path')
const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    },
    module: {
        rules: [
            {
                test: /\.s[ac]ss$/,
                use: ['style-loader','css-loader','saas-loader']
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            }
        ]
    }
}
module.exports = config
```
## 5 Extract CSS to a file
>ExtractTextPlugin is a webpack plugin that helps to move all the *.css classes & selectors into a separate .css file. Using style-loader will only inline the css classes but extract plug in helps to extract those css classes to a separate physical file improving the overall performance as the CSS files will be loaded by the browser in parallel to JS files.

Download the plugin from NPM library
```sh
npm install --save-dev extract-text-webpack-plugin
```
_webpack.config.js_
```javascript
const path = require('path')
const ExtractTextPlugin = require("extract-text-webpack-plugin")
const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    },
    module: {
        rules: [
            {
                test: /\.s[ac]ss$/,
                use: ExtractTextPlugin.extract({
                    use: ['style-loader','css-loader','saas-loader'],
                    fallback: 'style-loader'
                })
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin("styles.css")
    ]
}
module.exports = config
```
## 6. Stripping Unused CSS
>PurifyCSSPlugin is a webpack plugin that helps to remove the unused CSS selectors from the .css file. This has to be used along with the ExtractTextPlugin.

### 6.1. Installation
```sh
npm install --save-dev purifycss-webpack purify-css
```
### 6.2. Configuration
_webpack.config.js_
```javascript
const path = require('path')
const glob = require('glob')
const ExtractTextPlugin = require("extract-text-webpack-plugin")
const PurifyCSSPlugin = require("purifycss-webpack")

const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    },
    module: {
        rules: [
            {
                test: /\.s[ac]ss$/,
                use: ExtractTextPlugin.extract({
                    use: ['style-loader','css-loader','saas-loader'],
                    fallback: 'style-loader'
                })
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin("styles.css"),
        new PurifyCSSPlugin({
            paths: glob.sync(path.join(__dirname,'index.html'))
        })
    ]
}
module.exports = config
```
## 7. Using the Babel Loader
> **Babel** is a transpiler used to convert ES6 javascript code to the browser compatible code. In general transpilers are the ones which transform the code in one source language to another source language. 
### 7.1. Install Babel Loader
```sh
npm install --save-dev babel-loader babel-core
```
_webpack.config.js_
```javascript
const path = require('path')
const glob = require('glob')
const ExtractTextPlugin = require("extract-text-webpack-plugin")
const PurifyCSSPlugin = require("purifycss-webpack")

const config = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname,'dist')
    },
    module: {
        rules: [
            {
                test: /\.s[ac]ss$/,
                use: ExtractTextPlugin.extract({
                    use: ['style-loader','css-loader','saas-loader'],
                    fallback: 'style-loader'
                })
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            },
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin("styles.css"),
        new PurifyCSSPlugin({
            paths: glob.sync(path.join(__dirname,'index.html'))
        })
    ]
}
module.exports = config
```
### 7.2 Install Babel Presets
Babel just like any compiler goes through 3 stages: parsing, transforming and generation. To trigger the transformation appropriate plugins should be added. Here ES2015 plug in is added to trasnpile ES2015 code to the browser compatible code.
```sh
npm install --save-dev babel-preset-es2015
```
_.babelrc_
```json
{
    "presets": ["es2015"]
}
```
Using Babel preset without the .babelrc file
_webpack.config.js_
```javascript
module.exports = {
  // …
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: [/node_modules/],
        use: [{
          loader: 'babel-loader',
          options: { presets: ['es2015'] },
        }],
      },
    
      // Loaders for other file types can go here
    ],
  },
  // …
};
```

## 8. Working with Multiple Files
Multiple entry files can be configured by creating and entry object with multiple key/value pairs for each of the entry points.When using multiple entry points you must override the default output.filename option. Otherwise each entry point would write to the same output file. Use [name] to get the name of the entry point.
```javascript
{
    entry: {
        home: "./home",
        about: "./about",
        help: ["./help", "./manual"]
    },
    output: {
        path: path.join(__dirname, "dist"),
        filename: "[name].main.js"
    }
}
```
### 8.1 Multiple Files bundled together
The multiple entry files will be bundled together with as one output bundle file
```javascript
const path = require('path');
const webpack = require('webpack');
module.exports = {
  context: path.resolve(__dirname, './src'),
  entry: {
    app: ['./home.js', './events.js', './vendor.js'],
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].bundle.js',
  },
};
```
### 8.2 Multiple Files, Multiple Outputs
This will be bundled as 3 files: dist/home.bundle.js, dist/events.bundle.js, and dist/contact.bundle.js.
```javascript
const path = require('path');
const webpack = require('webpack');
module.exports = {
  context: path.resolve(__dirname, './src'),
  entry: {
    home: './home.js',
    events: './events.js',
    contact: './contact.js',
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].bundle.js',
  },
};
```
## 9. Code Splitting
### 9.1 Manual Splitting
```javascript
module.exports = {
  entry: {
    index: './index.js',
    vendor: ['react', 'react-dom', 'momento'],
  },
  // …
}
```
### 9.2 Automatic Splitting
```javascript
module.exports = {
  // …
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      name: 'commons',
      filename: 'commons.js',
      minChunks: 2,
    }),
  ],
// …
};
```
## 10. Plugins
### 10.1 HTMLWebpackPlugin
__webpack.config.js__
```javascript
const HTMLWebpackPlugin = require('html-webpack-plugin');
const HTMLWebpackPluginConfig = new HTMLWebpackPlugin({
    template: 'src/index.html',
    title: 'Webpack Tutorial',
    filename: 'index.html'
});
const path = require('path');

const config = {
    entry: './src/index/js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist');
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: 'babel-loader'
            }
        ]
    },
    plugins: [HTMLWebpackPluginConfig]
}
module.exports = config
```
__src/index.html__
```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-type" content="text/html; charset=utf-8"/>
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
  </body>
</html>
```









