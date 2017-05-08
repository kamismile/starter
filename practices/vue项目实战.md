[TOC]

# vue项目实战

## 环境搭建

```shell
vue init webpack zorro-ops
cd zorro-ops
npm install
npm install --save iview
mkdir src/api src/state
```

目录结构类似如下

```shell
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── task.js       # 任务管理模块
        └── offline.js    # 离线任务模块
```

代理: 与后台的交互后台使用spring boot因此借助webpack的代理功能将交互部分转发给后台

```diff
# config/index.js

proxyTable: {
+	'/api': {
+      target: 'http://localhost:3000',
+     // changeOrigin: true,
+      pathRewrite: {
+        '^/api': ''
+      }
	}
}
```



## 页面开发

### 模块 + 路由 + 布局

```shell
├── manage 	# 管理模块
│   ├── Binlog.vue 			# binlog管理
│   ├── Database.vue		# 数据库实例管理
│   └── offline.vue			# 离线管理
├── monitor # 监控模块

```

```javascript
// 路由规则 src/router/index.js
import Vue from 'vue'
import Router from 'vue-router'
import Binlog from '@/components/Binlog'
import Database from '@/components/Database'
import Offline from '@/components/Offline'
import Offset from '@/components/Offset'
import Server from '@/components/Server'
import Task from '@/components/Task'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'index',
      component: Task
    }, {
    	path: '/binlog', # 注意'/'不可省略
    	component: Binlog
    }, {
    	path: '/database',
    	component: Database
    }, {
    	path: '/offline',
    	component: Offline
    }, {
    	path: '/offset',
    	component: Offset
    }, {
    	path: '/server',
    	component: Server
    }, {
    	path: '/task',
    	component: Task
    }
  ]
})
```

```javascript
// main.js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
import iView from 'iview'
import 'iview/dist/styles/iview.css'

Vue.config.productionTip = false

Vue.use(iView)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: { App }
})
```



```html
<!-- App.vue-->
<template>
  <div class="layout" :class="{'layout-hide-text': spanLeft < 5}">
      <Row type="flex">
          <i-col :span="spanLeft" class="layout-menu-left">
              <Menu theme="dark" width="auto" @on-select="sidebarClick" :open-names="['1']" accordion>
                  <div class="layout-logo-left"></div>
                  <Submenu name="1">
                      <template slot="title">
                          <Icon type="settings"></Icon>
                          <span class="layout-text">任务管理</span>
                      </template>
                      <Menu-item name="task">
                          <Icon type="ios-analytics" :size="iconSize"></Icon>
                          <span class="layout-text">任务配置</span>
                      </Menu-item>
                      <Menu-item name="task2">
                          <Icon type="ios-analytics" :size="iconSize"></Icon>
                          <span class="layout-text">任务配置2</span>
                      </Menu-item>
                  </Submenu>
                  <Submenu name="2">
                      <template slot="title">
                          <Icon type="stats-bars"></Icon>
                          <span class="layout-text">监控</span>
                      </template>
                      <Menu-item name="2-1">
                          <Icon type="arrow-graph-up-right" :size="iconSize"></Icon>
                          <span class="layout-text">统计信息</span>
                      </Menu-item>
                  </Submenu>
                  
              </Menu>
          </i-col>
          <i-col :span="spanRight">
              <div class="layout-header">
                  <i-button type="text" @click="toggleClick">
                      <Icon type="navicon" size="32"></Icon>
                  </i-button>
              </div>
              <div class="layout-content">
                  <div class="layout-content-main">
                      <router-view></router-view>
                  </div>
              </div>
              <div class="layout-copy">
                  2017 &copy; Core
              </div>
          </i-col>
      </Row>
  </div>  
</template>

<script>
import router from './router'
export default {
  data () {
      return {
        spanLeft: 5,
        spanRight: 19
      }
  },
  computed: {
      iconSize () {
          return this.spanLeft === 5 ? 14 : 24;
      }
  },
  methods: {
      toggleClick () {
          if (this.spanLeft === 5) {
              this.spanLeft = 2;
              this.spanRight = 22;
          } else {
              this.spanLeft = 5;
              this.spanRight = 19;
          }
      },
      sidebarClick(name) {
          router.push(name)
      }
  }
}
</script>

<style>
.layout, .ivu-row-flex, .ivu-col-span-19 {
    height: 100%;
}
.layout{
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative; 
    border-radius: 4px;
    overflow: hidden;
}
.ivu-col-span-19 {
    overflow: auto;
}
.layout-breadcrumb{
    padding: 10px 15px 0;
}
.layout-content{
    min-height: 200px;
    /* height: 80%; */
    margin: 15px;
    overflow: hidden;
    background: #fff;
    border-radius: 4px;
}
.layout-content-main{
    padding: 10px;
}
.layout-copy{
    text-align: center;
    /* padding: 10px 0 20px; */
    bottom: 15px;
    color: #9ea7b4;
    position: fixed;
    width: 75%;
}
.layout-menu-left{
    background: #464c5b;
}
.layout-header{
    height: 60px;
    background: #fff;
    box-shadow: 0 1px 1px rgba(0,0,0,.1);
}
.layout-logo-left{
    width: 90%;
    height: 30px;
    background: #5b6270;
    border-radius: 3px;
    margin: 15px auto;
}
.layout-ceiling-main a{
    color: #9ba7b5;
}
.layout-hide-text .layout-text{
    display: none;
}
.ivu-col{
    transition: width .2s ease-in-out;
}
</style>
```

防止报错  mock Task.vue

```html
// components/Task.vue
<template>

</template>
<script>
  export default {
    
  }
</script>
<style scope>

</style>
```

样式调整

```diff
# index.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>zorro-ops</title>
+    <style>
+    	html, body {
+    		height: 100%;
+    	}
+    </style>
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

### 状态管理 + 模块开发

```shell
cd store
mkdir modules
touch actions.js getters.js index.js mutation-types.js modules/task.js
cd ../api
```



```javascript
// store/index.js
import Vue from 'vue'
import Vuex from 'vuex'
import * as actions from './actions'
import * as getters from './getters'

export default new Vuex.Store({
	actions,
	getters,
	modules: {

	}
})
```



```javascript
// store/actions.js
```

```javascript
// store/mutations.js
```

```javascript
// store/mutation-types.js
```

```javascript
// store/modules/task.js

```

```html
<!-- components/Task.vue -->

```

[购物车示例](https://github.com/vuejs/vuex/tree/dev/examples/shopping-cart)

**经典vuex用法解析:**

1. 页面加载完成后显示后端数据(元素节点创建后 回调事件dispatch action)

   显示用computed,查询用created

   ```html
   <!-- ProductList.vue -->
   <template>
   	....
   </template>
   <script>
   	export default {
         
         computed: mapGetters({
           products: 'allProducts'
         }),
         created () {
           this.$store.dispatch('getAllProducts');
         }
        
   	}
   </script>
   ```

   ​

2. 模块内部的action, mutation和getter现在仍然注册在全局命名空间，这样保证了多个模块能够响应同一个mutation和action.

      ```javascript
         // type.js
      ```

   // 定义getter, action, mutation的名称为常量，以模块名todos为前缀
   export const DONE_COUNT = 'todos/DONE_COUNT'
   export const FETCH_ALL = 'todos/FETCH_ALL'
   export const TOGGLE_DONE = 'todos/TOGGLE_DONE'

   // modules/todos.js
   import * as types from '../types'

   // 使用添加了前缀的名称定义 getter, action 和 mutation
   const todosModule = {
     state: {todos: []},
     getters: {
       [types.DONE_COUNT] (state) {
         // ...
       }
     },
     actions: {
       [types.FETCH_ALL] (context, payload) {
         // ...
       }
     },
     mutations: {
       [types.TOGGLE_DONE] (state, payload) {
         
       }
     }
   }
      ```

3.  state模块内的action调用api与后端交互，并在回调函数中进行mutations的commit操作

   ```javascript
   // store/modules/products.js

   import shop from '../../api/shop'
   import * as types from '../mutation-types'

   // initial state
   const state = {
     all: []
   }

   // getters
   const getters = {
     allProducts: state => state.all
   }

   // actions
   const actions = {
     getAllProducts ({ commit }) {
       shop.getProducts(products => {
         commit(types.RECEIVE_PRODUCTS, { products })
       })
     }
   }

   // mutations
   const mutations = {
     [types.RECEIVE_PRODUCTS] (state, { products }) {
       state.all = products
     },
     
     [types.ADD_TO_CART] (state, { id }) {
       state.all.find(p => p.id === id).inventory--
     }
   }

   export default {
     state,
     getters,
     actions,
     mutations
   }
   ```

4. 模仿上述操作开发task相应功能，首先task api, 提供获取tasks功能，然后state mutations完成set state，actions,完成对api功能调用，actions绑定在table组件created之后

   ```shell
   npm install --save axios
   ```

   ```javascript
   // api/taskApi.js

   import  axios from 'axios'

   const _tasks_query = ''

   const _tasks = [{
                     "id": "02-647-2383",
                     "ip": "6db4:9b3:e8ad:1fb4:38d6:cde5:bd0d:e7b1",
                     "rules": "businesses-16644-retaliates",
                     "locks": 1
                   }, {
                     "id": "89-097-8013",
                     "ip": "1b0:98e3:29bc:db8d:5e42:a245:cc0a:19ec",
                     "rules": "insanity's-76537-imminently",
                     "locks": 2
                   }, {
                     "id": "34-882-8361",
                     "ip": "735:536b:b70:3f8a:e906:fb9e:de19:4785",
                     "rules": "showcasing-94732-optician's",
                     "locks": 3
                   }, {
                     "id": "25-177-3566",
                     "ip": "266:5b75:ef0a:33d7:7783:652e:bc2d:29f2",
                     "rules": "synopsis's-64753-disloyalty",
                     "locks": 30
                   }];

   export default {
   	getTasks (cb) {
   		/*axios.get(_tasks_query, {

   		}).then(function (resp) {
   			console.log(response);
   		}).catch (function (err) {
   			console.log(err);
   		});*/
   		setTimeout(() => cb(_tasks), 100)
   	}
   }
   ```

   ```diff
   # store/index.js
   import Vue from 'vue'
   import Vuex from 'vuex'
   +import tasks from './modules/tasks'

   + Vue.use(Vuex);

   export default new Vuex.Store({
   	modules: {
   +		tasks
   	}
   })
   ```

   ```diff
   // src/main.js
   + import store from './store'

   new Vue({
   ...
   +  store,
   ...
   })
   ```

   **坑**

   - 每个模块拥有自己的state, mutation, action, getters
   - 模块内部的action,mutation, getter仍然注册在全局命名空间中，除state
   - 对于模块内部的mutation和getter,接收的第一个参数是模块的局部状态
   - 组件中获取vuex状态
     - import store from './store' + store.state.*
     - Vue.use(Vuex) + this.$store.state.*
     - import { mapState } from 'vuex' + mapState({count: state => state.count, countAlias: 'count'})

   ```diff
   # index.html 移除样式迁入App.vue
   - <style>
   -	...
   -</style>

   # App.vue
   - <style scoped>
   + <style>
   + ...index.html style
   + th .ivu-table-cell {
   +  text-overflow: initial
   + }
   ```

###   前后端协同开发

​	spring boot 环境搭建

```xml
<!-- pom.xml -->

<properties>
	<java.version>1.7</java.version>
  	<maven.compiler.source>${java.version}</maven.compiler.source>
  	<maven.compiler.target>${java.version}</maven.compiler.target>
</properties>
	<dependencies>
		<dependency>
			<groupId>org.apache.zookeeper</groupId>
			<artifactId>zookeeper</artifactId>
			<exclusions>
				<exclusion>
					<artifactId>slf4j-api</artifactId>
					<groupId>org.slf4j</groupId>
				</exclusion>
				<exclusion>
					<artifactId>slf4j-log4j12</artifactId>
					<groupId>org.slf4j</groupId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>com.google.guava</groupId>
			<artifactId>guava</artifactId>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>1.4.0.RELEASE</version>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>repackage</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>io.spring.platform</groupId>
				<artifactId>platform-bom</artifactId>
				<version>2.0.8.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
```

配置项注入

```yaml
foo:
	bar1: tar1
	bar2: tar2

```

```java
@Data
class Foo {
  private String bar1;
  private String bar2;
}

@Configuration
class AppConfig {
  @Bean
  @ConfigurationProperties(prefix = "foo")
  Foo foo() {
    return new Foo();
  }
}
```

注解式校验

- Bean上配置校验规则
- controller等方法签名上加上@Validated注解

集成swagger api文档说明 [swagger ui doc](http://springfox.github.io/springfox/docs/current/)

```diff
<!-- pom.xml -->
+<dependency>
+    <groupId>io.springfox</groupId>
+    <artifactId>springfox-swagger2</artifactId>
+    <version>2.6.1</version>
+</dependency>
```

