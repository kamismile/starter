# webpack

[TOC]

## Getting Started

### Create a bundle

```shell
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install --save-dev webpack

./node_modules/.bin/webpack --help
.\node_modules\.bin\webpack --help
webpack --help # if installed globally
```

> 创建子目录 `app` 以及`index.js`文件

```javascript
// app/index.js
function component() {
  var element = document.createElement('div');
  element.innerHTML = _.join(['hello', 'webpack'], ' ');
  return element;
}
document.body.appendChild(component());
```

>在根目录下创建`index.html`文件

```html
<!--index.html -->
<html>
  <head>
    <title>webpack 2 demo</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="app/index.js"></script>
  </body>
</html>
```

- 如果依赖项缺失，或者顺序有误，应用不会起作用
- 如果依赖项被包含但并没有实际使用，那么浏览器仍必须下载许多无用代码

```shell
npm install --save lodash
```

```diff
# app/index.js
+ import _ from 'lodash';

function component() {
  ...
}
```

```diff
# index.html

<html>
	<head>
		<title>webpack 2 demo</title>
- 		<script src="https://unpkg.com/lodash@4.16.6"></script>		
	</head>
	<body>
-		<script src="app/index.js"></script>
+		<script src="dist/bundle.js"></script>
	</body>
</html>
```

```shell
./node_modules/.bin/webpack app/index.js dist/bundle.js
```



### Using ES2015 modules with webpack

### Using webpack with a config

```javascript
// webpack.config.js
var path = require('js');
module.exports = {
  entry: './app/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist');
  }
}
```

```shell
./node_modules/.bin/webpack --config webpack.config.js
```

### Using webpack with npm

```json
{
  "scripts": {
    "build": "webpack"
  }
}
```

```shell
npm run build -- --colors
```

## Installation

```shell
npm install --save-dev webpack
npm install --save-dev webpack@<version>
```

```json
"scripts": {
  "start": "webpack --config mywebpack.config.js"
}
```

> 本地运行webpack可使用node_modules/.bin/webpack

#### global installation

```shell
npm install --global webpack
```

## Migrating from v1 to v2

## Code Splitting

### Resource splitting for caching and parallel loads

### Vendor code splitting

> CommonsChunkPlugin

### CSS splitting

> ExtractTextWebpackPlugin

## Code Splitting - CSS

### Imporing CSS

```javascript
// import the css file like a javascript module, for instance in vendor.js
import 'bootstrap/dist/css/bootstrap.css';
```

### Using css-loader and style-loader

```shell
npm install --save-dev css-loader style-loader
```

这种方式会让css随javascript脚本加载而载入，会失去css异步动态载入的特性，页面会等待js加载完成才渲染

可以通过`ExtractTextWebpackPlugin`来单独操作CSS解决此问题

### Using `ExtraTextWebpackPlugin`

```shell
npm install --save-dev extract-text-webpack-plugin
```

```diff
# webpack.config.js
+var ExtractTextPlugin = require('extract-text-webpack-plugin');
module.exports = {
  modules: {
    rules: [{
      test: /\.css$/,
-	  use: [ 'style-loader', 'css-loader' ],
+	  use: ExtractTextPlugin.extract({
+  			use: 'css-loader'
+	  })
    }]
  },
+ plugins: [
+ 	new ExtractTextPlugin('styles.css'),  
+]  
}
```

## Code Splitting - Libraries

```shell
npm install --save moment
```

```javascript
// index.js
var moment = require('moment');
console.log(moment().format());

```

```javascript
// webpack.config.js
var path = require('path');

module.exports = function() {
  return {
    entry: './index.js',
    output: {
      filename: '[name].[chunkhash].js',
      path: path.resolve(__dirname, 'dist')
    }
  }
}
```

### Multiple Entries

```javascript
var path = require('path');

module.exports = function(env) {
  return {
    entry: {
      main: './index.js',
      vendor: 'moment'
    },
    output: {
      filename: '[name].[chunkhash].js',
      path: path.resolve(__dirname, 'dist')
    }
  }
}
```

### CommonsChunkPlugin

```javascript
// webpack.config.js
var webpack = require('webpack');
var path = require('path');

module.exports = function(env) {
  return {
    entry: {
      main: './index.js',
      vendor: 'moment'
    },
    output: {
      filename: '[name].[chunkhash].js',
      path: path.resolve(__dirname, 'dist')
    },
    plugins: [
      new webpack.optimize.CommonsChunkPlugin({
        name: 'vendor'
      })
    ]
  }
}
```

### Implicit Common Vendor Chunk

```javascript
var webpack = require('webpack');
var path = require('path');

module.exports = function() {
  return {
    entry: {
      main: './index.js'
    },
    output: {
      filename: '[name].[chunkhash].js',
      path: path.resolve(__dirname, 'dist')
    },
    plugins
  }
}
```



