# React Boilerplate - Minimal Configuration
### 1. Prepare the Environment
```sh
mkdir react-boilerplate
cd react-boilderplate
npm init -y
```
```sh
mkdir dist
mkdir src
cd src
touch index.html
```
```sh
npm install --save-dev webpack webpack-dev-server
```
_**package.json**_
```json
...

"scripts": {
	"start":"webpack-dev-server --progress -colors --hot"
	...
}
...
```
_**webpack.config.js**_
```javascript
module.exports = {
	entry: [
		'./src/index.js'
	],
	output: {
		path: __dirname + '/dist',
		publicPath: '/',
		filename: 'bundle.js'
	},
	devServer: {
		contentBase: './dist'
	}
};
```
```sh
cd src
touch index.js
```
_**index.js**_
```javascript
console.log('minimal setup')
```
### 2. Hot Reloading
```sh
npm install --save-dev react-hot-loader
```
_**webpack.config.js**_
```javascript
module.exports = {
	entry: [
		'webpack-dev-server/client?http://localhost:8000',
		'webpack/hot/only-dev-server',
		'./src/index.js'
	],
	output: {
		path: __dirname + '/dist',
		publicPath: '/',
		filename: 'bundle.js'
	},
	devServer: {
		contentBase: './dist',
		hot: true
	}
};
```
_**index.js**_
```sh
console.log("now loaded hot");
module.hot.accept();
```
### 3. Babel Installation
```sh
npm install --save-dev babel-core babel-loader babel-preset-es2015
npm install --save-dev babel-preset-state-2
npm install --save-dev babel-preset-react
```
_**package.json**_
```json
...
"license":"ISC",
"babel": {
	"presets": [
		"es2015",
		"react",
		"stage-2"
	]
}
...
```

_**webpack.config.js**_
```javascript
module.exports = {
	entry: [
		'webpack-dev-server/client?http://localhost:8000',
		'webpack/hot/only-dev-server',
		'./src/index.js'
	],
	output: {
		path: __dirname + '/dist',
		publicPath: '/',
		filename: 'bundle.js'
	},
	module: {
		loaders: [{
			test: /\.jsx?$/,
			exclude: /node_modules/,
			loader: 'react-hot-loader!babel-loader'
		}]
	},
	resolve: {
		extensions: ['*','.js','.jsx']
	},
	devServer: {
		contentBase: './dist',
		hot: true
	}
};
```
```sh
npm install --save react react-dom
```

_**src/index.js**_
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

const title = 'Minimal React Webpack Babel Setup';

ReactDOM.render(
	<div>{title}</div>
	document.getElementById('app')
);

module.hot.accept();
```
```sh
npm install --save-dev eslint
npm install --save-dev eslint-loader
```
_**webpack.config.js**_
```javascript
...
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: 'react-hot-loader!babel-loader'
    },
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'eslint-loader'
    }
  ]
},
...
```
```sh
touch .eslintrc
```
_**.eslintrc**_
```json
{
	"rules": {
	}
}
```
```sh
npm install --save-dev babel-loader
```
```javascript
...
module: {
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: 'react-hot-loader!babel-loader'
    },
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loaders: ['babel-loader', 'eslint-loader']
    }
  ]
},
...
```
 Alternatively use webpack way
```javascript
...
module: {
  preLoaders: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'eslint-loader'
    },
  ],
  loaders: [
    {
      test: /\.jsx?$/,
      exclude: /node_modules/,
      loader: 'react-hot-loader!babel-loader'
    }
  ]
},
...
```
```sh
npm install --save-dev babel-eslint
```
_**.eslintrc**_
```json
{
	parser: "babel-eslint",
	"rules": {
	}
}
```
### 4. Using Airbnb Style Guide
```sh
npm install --save-dev eslint-config-airbnb esling-plugin-import eslint-plugin-jsx-a11y
```
_**.eslintrc**_
```json
{
	parser: "babel-eslint",
	"extends": ["airbnb-base"]
}
```
