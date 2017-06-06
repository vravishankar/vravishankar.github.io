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
npm install --save-dev extract-text-webpack-plugin
```
## 5. Install Loaders
```sh
npm install --save-dev css-loader
npm install --save-dev style-loader
npm install --save-dev sass-loader
npm install --save-dev node-sass
npm install --save-dev json-loader
```
## 6. Install PostCSS and Normalize CSS
```sh
npm install --save-dev postcss-loader
npm install --save-dev precss
npm install --save-dev autoprefixer
npm install --save-dev normalize-css
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

// Plugins
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');

// PostCSS
const precss = require('precss');
const autoprefixer = require('autoprefixer');

// Environment Setup
const HOST = process.env.HOST || 'localhost';
const PORT = process.env.PORT || 8080;

module.exports = {
    entry: {
        app: '/src/app.js'
    },
    output: {
        path: 'bundle.js',
        filename: '[name].js'
    },
    resolve: {
        extensions: ['','.js','.jsx','.css']
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            },
            {
                test: /\.json$/,
                use: 'json-loader'
            },
            {
                test: /\.scss$/,
                use: ExtractTextPlugin.extract({
                    fallback: 'style-loader',
                    use: [
                        'style-loader', 
                        {
                            loader: 'css-loader',
                            options: {
                                importLoaders: 1
                            }
                        },
                        {
                            loader: 'postcss-loader',
                            options: {
                                plugins: function() {
                                    return [
                                        require("autoprefixer")
                                    ]
                                }
                            }
                        },
                        {
                            loader:'sass-loader'
                        }
                    ]
                })
            }
        ]
    },
    devServer: {
        contentBase: path.join(__dirname,"dist"),
        compress: true
    }
    plugins: [
        new ExtractTextPlugin('[name].css')
    ]
}
```
## 9. Configure webpack-dev-server
```json
...
"scripts": {
    "dev": "webpack-dev-server",
    "prod":"webpack -p"
}
...
```
## 10. Use rimraf for Clean Build
```sh
npm install --save-dev rimraf
```
```json
...
"scripts": {
    "dev": "webpack-dev-server",
    "prod":npm run clean && "webpack -p",
    "clean": "rimraf ./dist/*"
}
...
```
## 11. Hot Module Replacement
```javascript
const webpack = require('webpack');
const path = require('path');

// Plugins
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

// PostCSS
const autoprefixer = require('autoprefixer');

// Environment Setup
const HOST = process.env.HOST || 'localhost';
const PORT = process.env.PORT || 8080;

module.exports = {
    entry: {
        app: '/src/app.js'
    },
    output: {
        path: 'bundle.js',
        filename: '[name].js'
    },
    resolve: {
        extensions: ['','.js','.jsx','.css']
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            },
            {
                test: /\.json$/,
                use: 'json-loader'
            },
            {
                test: /\.scss$/,
                use: ExtractTextPlugin.extract({
                    fallback: 'style-loader',
                    use: [
                        'style-loader', 
                        {
                            loader: 'css-loader',
                            options: {
                                importLoaders: 1
                            }
                        },
                        {
                            loader: 'postcss-loader',
                            options: {
                                plugins: function() {
                                    return [
                                        require("autoprefixer")
                                    ]
                                }
                            }
                        },
                        {
                            loader:'sass-loader'
                        }
                    ]
                })
            }
        ]
    },
    devServer: {
        contentBase: path.join(__dirname,"dist"),
        hot: true,
        compress: true,
        publicPath: '/'
    }
    plugins: [
        new ExtractTextPlugin('[name].css'),
        new webpack.HotModuleReplacementPlugin() // Enable HMR
    ]
}
```
## 12. Production Environment
```json
...

"scripts": {
    "dev": "webpack-dev-server",
    "prod":"npm run clean && NODE_ENV=production webpack -p",
    "clean": "rimraf ./dist/*"
}
...
```
```javascript
const webpack = require('webpack');
const path = require('path');

// Plugins
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

// PostCSS
const autoprefixer = require('autoprefixer');

// Environment Setup
const HOST = process.env.HOST || 'localhost';
const PORT = process.env.PORT || 8080;

// Production Setup

var isProduction = process.env.NODE_ENV === 'production';
var cssDev = ['style-loader','css-loader','postcss-loader','sass-loader']
var cssProd = ExtractTextPlugin.extract({
    fallback: 'style-loader',
    loader: ['css-loader','postcss-loader','sass-loader'],
    publicPath: '/dist'
});
var cssConfig = isProduction ? cssProd : cssDev

module.exports = {
    entry: {
        app: '/src/app.js'
    },
    output: {
        path: 'bundle.js',
        filename: '[name].js'
    },
    resolve: {
        extensions: ['','.js','.jsx','.css']
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            },
            {
                test: /\.json$/,
                use: 'json-loader'
            },
            {
                test: /\.scss$/,
                use: ExtractTextPlugin.extract({
                    fallback: 'style-loader',
                    use: cssConfig
                })
            }
        ]
    },
    devServer: {
        contentBase: path.join(__dirname,"dist"),
        hot: true,
        compress: true,
        publicPath: '/'
    }
    plugins: [
        new HtmlWebpackPlugin({
            title: 'Project Template',
            hash: true,
            template: './src/index.html'
        }),
        new ExtractTextPlugin({
            filename: '[name].css',
            disable: !isProduction,
            allChunks: true
        }),
        new webpack.HotModuleReplacementPlugin() // Enable HMR
    ]
}
```
```html
<!DOCTYPE html>
<html>
    <head>
        <title><%= htmlWebpackPlugin.options.title %></title>
    </head>
    <body>
        <p>Hello Webpack</p>
    </body>
</html>
```


