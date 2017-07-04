[TOC]

# 环境搭建

```shell
mkdir intro
cd intro
npm init -y
npm install --save-dev webpack webpack-dev-server 

touch webpack.config.js
echo '''
const path = require('path')
module.exports = {
 entry: '/src/index.js',
 output: {
   filename: 'bundle.js',
   path: path.resolve(__dirname, 'dist')
 }
}
'''>webpack.config.js

# package.json
"script": {
  "start": "webpack-dev-server"
}

# index.html
# for webpack-dev-server
<script src="/bundle.js"></script>
# for prod
<script src="dist/bundle.js"></script>

npm run start
```

# 环境搭建二

```shell
mkdir reactApp
cd reactApp
npm init -y
npm install webpack --save-dev
npm install webpack-dev-serer --save-dev
npm install react react-dom --save
npm install babel-core babel-loader babel-preset-react babel-preset-es2015 --save-dev
touch index.html App.jsx main.js webpack.config.js

```

```javascript
// webpack.config.js
var config = {
  entry: './main.js',
  output: {
    path: './',
    filename: 'index.js'
  },
  devServer: {
    inline: true,
    port: 8080
  },
  module {
    loaders: [
      {
        test: /\.jsx?$/,
  		exclude: /node_modules/,
  		loader: 'babel-loader',
  
  		query: {
          presets: ['es2015', 'react']
  		}
      }
    ]
  }
}

module.exports = config;


// package.json
"start": 'webpack-dev-server --hot'


```

```html
<html>
  ...
  <body> 
	<div id='app'></div>
    <script src="index.js"></script>
  </body>
  ...
</html>

```

```javascript
// App.jsx
import React from 'react';

class App extends React.Component {
  render() {
    return (
    	<div>
      		Hello World!
      	</div>
    )
  }
}

export default App;

// main.js
import React from 'react';
import ReactDom from 'react-dom';
import App from './App.jsx';

ReactDom.render(<App />, document.getElementById('app'));


```

```shell
npm start
```



