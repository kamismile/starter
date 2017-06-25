# 指定日志文件路径

以system property形式指定logback.configurationFile

# 占位符加el表达式指定属性

用法1： 指定文件路径,日志级别

```shell
java -jar *.jar -Dfoo=fooValue
```

日志文件中配置中用*${foo}*的占位符形式配置

# 配置文件修改后自动重新加载

```xml
<configuration scan="true">
...
</configuration>
```

默认情况下会每隔一分钟扫描一次变动可以指定**scanPeriod**, 单位可以是毫秒、秒、分钟或小时

```xml
<configuration scan="true" scanPeriod="30 seconds">
...
</configuration>
```

# 判断系统类型

commons-lang3 SystemUtils中可得到

# 运行命令程式

common-exec

win: bat start /java -jar -DprocessName=foo

查找相关程式 jps -v / tasklist tasklist查看得到的都是java.exe进程，这里用jps -v可看到完整的带参数的命令，通过指定的名称可定义程式用来停止

taskill

```powershell
START "do something window" dir
FOR /F "tokens=2" %I in ('TASKLIST /NH /FI "WINDOWTITLE eq do something window"' ) DO SET PID=%I
ECHO %PID%
TASKKILL /PID %PID%
```

```java
private static final String TASKLIST = "tasklist";
private static final String KILL = "taskkill /F /IM ";

public static boolean isProcessRunning(String serviceName) throws Exception {

 Process p = Runtime.getRuntime().exec(TASKLIST);
 BufferedReader reader = new BufferedReader(new InputStreamReader(
   p.getInputStream()));
 String line;
 while ((line = reader.readLine()) != null) {

  System.out.println(line);
  if (line.contains(serviceName)) {
   return true;
  }
 }

 return false;

}

public static void killProcess(String serviceName) throws Exception {

  Runtime.getRuntime().exec(KILL + serviceName);

 }
```





# webpack basics

```shell
yarn add --save babel-core babel-loader babel-preset-es2015 babel-preset-react babel-preset-stage-2 webpack webpack-dev-serer
yarn add react react-dom prop-types
```

```javascript
// package.json
"scripts": {
  "start": "npm run build",
  "build": "webpack -d && cp src/index.html dist/index.html && webpack-dev-server --content-base src/ --inline --hot",
  "build:prod": "webpack -p && cp src/index.html dist/index.html"
}
```

```javascript
// webpack.config.js
var webpack = require('webpack');
var path = require('path');

var DIST_DIR = path.resolve(__dirname, 'dist');
var SRC_DIR = path.resolve(__dirname, 'src');

var config = {
  entry: SRC_DIR + '/app/index.js',
  output: {
    path: DIST_DIR + '/app',
    filename: 'bundle.js',
    publicPath: '/app/'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        include: SRC_DIR,
        loader: 'babel-loader',
        query: {
          presets: ['react', 'es2015', 'stage-2']
        }
      }
    ]
  }
}

module.exports = config;
```



# create components

```jsx
import React from 'react';
import { render } from 'react-dom';

class App extends React.Component {
  render() {
    return (
    	<div className="container">
      		<div className="row">
        		<div className=".col-xs-10 col-xs-offset-1">
              		<h1>hello!</h1>
              	</div>
        	</div>
      	</div>
    )
  }
}

render(<App />, document.getElementById('app'));
```

# create multiple components

```jsx
import React from 'react';

export class Header extends React.Component {
  render() {
    return (
    	<nav className="navbar navbar-default">
          <div className="container">
            <div className="navbar-header">
              <ul className="nav navbar-nav">
                <li><a href="#">Home</a></li>
              </ul>
            </div>
          </div>
      </nav>
    )
  }
}
```

# dynamic data

```jsx
import React from 'react';

export class Home extends React.Component {
  render() {
    let content = "";
    if (true) {
      content = <h1>hello</h1>;
    }
    return (
    	<div>
      		<p>hello this is a component</p>
        	{ 2 + 2}
        	{ content }
        	{ 5 === 2 ? 'yes': 'no'}
      	</div>
    )
  }
}
```

# passing data with props

```jsx
// index.js
import React from 'react';
import { render } from 'react-dom';

import { Header } from './components/Header';
import { Home } from './components/Home';

class App extends React.Component {
  render() {
    var user = {
      name: 'anna',
      hobbies: ['sports']
    }
    return (
    	<div>
      		<Home name={'dewey'} age={27} user={user}>
      			<p>this is a paragraph</p>
      		</Home>
      	</div>
    )
  }
}

render(<App />, document.getElementById('app'));

// Home.js

import React from 'react';

export class Header extends React.Component {
  render() {
    console.log(this.props)
    return (
    	<p>In a new Component </p>
      	<p> your name is {this.props.name}, your age is {this.props.age} </p>
      <p>User object => name: {this.props.user.name} </p>
      <div>
      	<h4>Hobbies</h4>
      	<ul>
      		{this.props.user.hobbies.map((hobby, i) => <li key={i}>{hobby}</li>)}
      	</ul>
      </div>
      <hr />
      {this.props.children}
    )
  }
}

Home.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number,
  user: PropTypes.object,
  children: PropTypes.element.isRequired
}

```

# events

```jsx
import React from 'react';
import PropTypes from 'prop-types';

export class Home extends React.Component {
  constructor(props) {
    super();
    this.age = props.age;
  }
  
  onMakeOlder() {
    this.age += 3;
  }
  render() {
    <div>
      <p>your name is {this.props.name}, your age is {this.props.age}, {this.age}</p>
      <button onClick={this.onMakeOlder.bind(this)} className="btn btn-primary">make me older</button>
      <button onClick={() => this.onMakeOlder()} className="btn btn-primary">make me older</button>
    </div>
  }
}

Home.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number,
  user: PropTypes.object,
  children: PropTypes.element.isRequired
}
```

# state

```jsx
import React from 'react';
import PropTypes from 'prop-types';

export class Home extends React.Component {
  constructor(props) {
    super();
    this.state = {
      age: props.age,
      status: 0
    }
  }
  onMakeOlder() {
    this.setState({
      age: this.state.age + 3
    })
  }
  render() {
    console.log(this.props);
    return (
    	<div>
      		<p>In a new Component </p>
        	<p>your name is {this.props.name}, your age is {this.state.age}</p>
        	<p>status: {this.status.status}</p>
        	<button onClick={() => this.onMakeOlder()} className="btn btn-primary">make me older</button>
      	</div>
    )
  }
}

Home.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number,
  user: PropTypes.object,
  children: PropTypes.element.isRequired
}
```

# how update the dom

virtual dom

# communicating between parent and children components

```jsx
import React from 'react';
import { render } from 'react-dom';

import Home from './components/Home';

class App extends React.Component {
  onGreet() {
    alert('greet');
  }
  render() {
    return (
    	<div>
      		<Home name={"dewey"} initialAge={27} greet={this.ongreet} />
      	</div>
    )
  }
}

render(<App />, document.getElementById('app'));

// home
import React from 'react';
import PropTypes from 'prop-types';

export class Home extends React.Component {
  render() {
    return (
    	<div>
      		<p> your name is {this.props.name}, your age is {this.props.initialAge}</p>
        <button onClick={this.props.greet} className="btn btn-primary">greet</button>
      	</div>
    )
  }
}

Home.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number,
  greet: PropTypes.func
}
```

# passing data between parent and children component

# two-way binding

# component lifecycle

# router





---

