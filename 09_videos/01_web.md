





[TOC]

# 06 React Router 4.0 with the real world example ! - react js course!





# 05 react fundamentals: 31 - why react? an overview of react, webpack 2, React Router v4, and babel.

```javascript
// webpack.config.js
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './app/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'index_bundle.js'
  },
  module: {
    rules: [
      { test: /\.(js)$/, use: 'babel-loader' },
      { test: /\.css$/, use: [ 'style-loader', 'css-loader' ]}
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'app/index.html'
    })
  ]
}
```

```json
"babel": {
  "presets": [
    "env",
    "react"
  ]
}
```

```javascript
// axios
function getRepos(username) {
  return axios.get('http://api.github.com/users/' + username + '/repos' + param + '&per_page=100')
}
```



# 04 react fundamentals: #2 - building your first react component with babel and webpack 2

```shell
npm install -g create-react-app
mkdir github-battle
cd github-battle
npm init -y
npm install --save react react-dom
touch .gitignore
```

```properties
# node_modules
node_modules
```

```shell
mkdir app
cd app
touch index.css index.js

```

```css
/* index.css */
body {
  background: green;
}
```

```jsx
// index.js
var React = require('react');
var ReactDOM = require('react-dom');
require('./index.css');

class App extends React.Component {
  render() {
    return (
    	<div>
      		hello world!
      	</div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('app'));
```

```shell
npm install --save-dev babel-core babel-loader babel-preset-env babel-preset-react css-loader style-loader html-webpack-plugin webpack 
```

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './app/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'index_bundle.js'
  },
  module: {
    rules: [
      { test: /\.(js)$/, use: 'babel-loader' },
      { test: /\.(js)$/, use: [ 'style-loader', 'css-loader' ]}
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'app/index.html'
    })
  ]
}
```

babel设置通过package.json或webpack.config.js取代.babelrc

```json
// package.json
"babel": {
  "presets": ["env", "react"]
},
"scripts": {
  "create": "webpack"
}
```

```html
<!-- touch index.html -->
<html>
  <head>
    <meta charset="UTF-8">
    <title>Github Battle</title>
  </head>
  <body>
    <div id="app">
      
    </div>
  </body>
</html>
```

```shell
npm install --save-dev webpack-dev-server
```

```diff
# package.json
"scripts": {
-  "create": "webpack",
+  "start": "webpack-dev-server --open" 
}
```



# 03 webpack 2 - how to install react and babel



```shell
mkdir webpack-101
cd webpack-101
yarn init -y
yarn add --dev webpack babel-core babel-loader
touch webpack.config.js
# npm i -D babel babel-preset-react babel-preset-es2015
```

在babel官网有关于如何集成webpack及其它工具的方式及命令

```shell
touch .babelrc
```

```json
// .babelrc
{
  "presets": ["es2015", "react"]
}
```

```shell
mkdir src
touch src/app.js app.scss index.html

```

```jsx
// app.js
const css = require('./app.scss');
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
	<h1>Hello, World</h1>,
  	document.getElementById('root')
)
```

```html
<!-- index.html -->
<html>
  <head>
    <meta charset="UTF8">
    <title></title>
  </head>
  <body>
    <div id='root'>
      
    </div>
  </body>
</html>
```

```shell

```



```javascript
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './src/app.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'app.bundle.js'
  },
  module: {
    rules: [{
      test: /\.scss$/,
      use: ExtractTextPlugin.extract({
        fallbackLoader: 'style-loader',
        loader: ['css-loader', 'scss-loader'],
        publicPath: '/dist'
      })
    }, {
      test: /\.js$/,
      excluce: /node_modules/,
      use: 'babel-loader'
    }]
  },
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    compress: true,
    stats: 'errors-only',
    open: true
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'project demo',
      
    })
  ]
}
```





# 02 getting started with react, babel and webpack in 2017

```shell
mkdir hisper
cd hisper
yarn init -y
yarn add --dev bael-loader babel-core babel-preset-es2015 babel-preset-react webpack
yarn add react react-dom
npm install -g babel
touch webpack.config.js
```

```javascript
var path = require('path');
var webpack = require('webpack');

module.exports = {
  entry: './app.js',
  output: { path: __dirname, filename: 'bundle.js'},
  
  module: {
    loaders: [
      {
        test: /.jsx?$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['es2015', 'react']
        }
      }
    ]
  }
}
```

```shell
touch app.js

```

```jsx
// app.js

import React from 'react';
import ReactDOM from 'react-dom';

var CommentBox = React.createClass({
  render: function() {
    return (
    	<div className="commentBox">
      		hello, world I am a comment box;
      	</div>
    )
  }
});

ReactDOM.render(
	<CommentBox />,
  	document.getElementById('root');
)
```

```json
// 这种方式可以不用全局安装webpack, babel, 且使用的是项目中的对应版本
"scripts": {
  "webpack": "webpack",
  "babel": "babel"
}
```









# 01 webpack babel

```shell
mkdir myapp
cd myapp
yarn init -y
npm install webpack
touch people.js app.js
```

```javascript
// people.js
const people = ['bob', 'joe', 'mary', 'john'];

module.exports = people;
```

```javascript
// app.js
import people from './people';

console.log(people);
```

控制台直接使用webpack 打包多个js文件示例

```shell
npm install -g webpack
webpack app.js app.bundle.js
node app.bundle.js
```

集成webpack到项目中

```shell
yarn add --dev webpack
touch webpack.config.js
mkdir src
mv app.js src/
mv people.js src/
```

```javascript
// webpack.config.js
module.exports = {
  entry: './src/app.js',
  output: {
    path: __dirname + '/dist',
    filename: 'app.bundle.js'
  }
}

```

```shell
webpack
node app.bundle.js
# 集成babel
yarn add --dev babel-loader babel-core babel-preset-es2015
touch .babelrc
```

```json
// .babelrc
{
  "presets": ["es2015"]
}
```

```javascript
// webpack.config.js
module.exports = {
  entry: './src/app.js',
  output: {
    path: __dirname + '/dist',
    filename: 'app.bundle.js'
  },
  module: {
    loaders: [
      {
        exclude: /node_modules/,
        loader: 'babel-loader'
      }
    ]
  }
}
```



