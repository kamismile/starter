





[TOC]

# 03 webpack 2 - how to install react and babel



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



