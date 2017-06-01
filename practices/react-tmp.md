[TOC]

# ant design学习使用

参考 [ant design](https://ant.design/index-cn)

参考 [12步30分钟，完成用户管理的CURD应用](https://github.com/sorrycc/blog/issues/18)

参考 [项目实战](https://ant.design/docs/react/practical-projects-cn)

## 原生方式构建项目 step-by-step

参考 [webpack官网](https://webpack.js.org/concepts/)

参考 [webpack中文教程](http://zhaoda.net/webpack-handbook/what-is-webpack.html)

```shell
# 创建目录初始化工程
mkdir intro-antd && cd intro-antd
npm init -y
# 安装webpack
npm install --save-dev webpack webpack-dev-server
# 创建webpack配置文件
touch webpack.config.js
# 创建简单的静态文件
touch index.html
mkdir src && cd src
touch index.js
cd ..

```

```javascript
// webpack.config.js
var path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

```diff
// package.json
  "scripts": {
-    "test": "echo \"Error: no test specified\" && exit 1"
+   "dev": "webpack-dev-server --port 8080",
+ 	"build": "webpack"
  },
```

```html
<!-- index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>

  <script src="/bundle.js"></script>
</body>
</html>
```

```javascript
// index.js
document.write('<h1>hello test</h1>');
```

```shell
# 启动
npm run dev 
# 访问http://localhost:8080/index.html
```

如果在*vagrant*中使用参考 [development-vagrant](https://webpack.js.org/guides/development-vagrant/)

```shell
# 安装ant design库
npm install --save antd react react-dom
```

```diff
# index.html

...
<body>

+ <div id="root"></div>
  <script src="/bundle.js"></script>
</body>
...
```

加入es6的 import等语法支持

```shell
npm install --save-dev babel-loader babel-core
```

修改**webpack.config.js**

```json
var path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    loaders: [
      {test: /\.js$/, loader: 'babel-loader'}
    ]
  }
}

```

当然为了支持jsx的语法,需要再安装一些插件

```shell
npm install --save-dev babel-preset-react babel-preset-es2015
```

修改**webpack.config.js**

```javascript
var path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: [ 'react', 'es2015']
        }
      },
    ]
  }
}
```

上面多次安装插件babel-loader babel-core, babel-preset-react babel-preset-es2015等和修改webpack.config.js可合并

修改index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { DatePicker, message } from 'antd';
import 'antd/dist/antd.css';

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      date: ''
    }
  }
  handleChange(date) {
    message.info('日期: ' + date.toString());
    this.setState({ date });
  }
  render() {
    return (
      <div style={{ width: 400, margin: '100px auto' }}>
        <DatePicker onChange={value => this.handleChange(value)} />
        <div style={{ marginTop: 20 }}>当前日期: {this.state.date.toString()}</div>
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

查看http://localhost:8080/index.html发现可以使用，但样式错误，因为压根没有引入样式，现在引入样式

```shell
npm install --save-dev css-loader style-loader
```

编辑webpack.config.js

```diff
....	
  module: {
    loaders: [
      {
        test: /\.jsx?$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: [ 'react', 'es2015']
        }
-     }  
+     }, {
+        test: /\.css$/,
+        loader: 'style-loader!css-loader' // 同时使用两种loader
+      }
    ]
  }
....  
```

现在应该可以正常显示，但控制台会有提示`bundle.js:66999 You are using a whole package of antd, please use https://www.npmjs.com/package/babel-plugin-import to reduce app bundle size.`

按提示安装模块化引入插件 [坑1]

```shell
npm install --save-dev babel-plugin-import
```

修改index.js

```javascript
import { Layout, Menu, Icon } from 'antd';
const { Header, Content, Footer, Sider } = Layout;
import React from 'react';
import ReactDOM from 'react-dom';
import 'antd/dist/antd.css';
import './index.css';

class App extends React.Component {
  render() {
    return (
      <Layout>
        <Sider
          breakpoint="lg"
          collapsedWidth="0"
          defaultCollapsed={false}
        >
          <div className="logo" />
          <Menu theme="dark" mode="inline" defaultSelectedKeys={['4']}>
            <Menu.Item key="1">
              <Icon type="user" />
              <span className="nav-text">nav 1</span>
            </Menu.Item>
            <Menu.Item key="2">
              <Icon type="video-camera" />
              <span className="nav-text">nav 2</span>
            </Menu.Item>
            <Menu.Item key="3">
              <Icon type="upload" />
              <span className="nav-text">nav 3</span>
            </Menu.Item>
            <Menu.Item key="4">
              <Icon type="user" />
              <span className="nav-text">nav 4</span>
            </Menu.Item>
          </Menu>
        </Sider>
        <Layout>
          <Header style={{ background: '#fff', padding: 0 }} />
          <Content style={{ margin: '24px 16px 0' }}>
            <div style={{ padding: 24, background: '#fff', minHeight: 360 }}>
              content
            </div>
          </Content>
          <Footer style={{ textAlign: 'center' }}>
            Ant Design ©2016 Created by Ant UED
          </Footer>
        </Layout>
      </Layout>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('root'));

```

index.css

```css
 .logo {
  height: 32px;
  background: #333;
  border-radius: 6px;
  margin: 16px;
}
html, body, #root {
  height: 100%;
}

```







## create-react-app方式构建



## 使用dva构建项目

### 安装dva-cli

```shell
$ npm i -g dva-cli
$ dva -v
0.7.8
```

### 创建新应用

```shell
dva new antd-quickstart
cd antd-quickstart
npm start
```

### 使用antd

```shell
npm i --save antd prop-types
npm i --save-dev babel-plugin-import 
```



编辑`.roadhogrc` , 使`babel-plugin-import`插件生效

```diff
{
  "entry": "src/index.js",
  "env": {
    "development": {
      "extraBabelPlugins": [
        "dva-hmr",
        "transform-runtime",
+        ["import", { "libraryName": "antd", "style": "css" }]
      ]
    },
    "production": {
      "extraBabelPlugins": [
        "transform-runtime",
+        ["import", { "libraryName": "antd", "style": "css" }]
      ]
    }
  }
}
```

### 创建布局

参考 [layout](https://ant.design/components/layout-cn/)





### 定义路由(pages)





### 编写UI Component

### 定义Model

### connect起来

### 后端开发

### 后端测试

### 代理

### 构建应用

