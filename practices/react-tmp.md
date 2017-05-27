[TOC]

# ant design项目实战

参考 [ant design](https://ant.design/index-cn)

参考 [12步30分钟，完成用户管理的CURD应用](https://github.com/sorrycc/blog/issues/18)

参考 [项目实战](https://ant.design/docs/react/practical-projects-cn)

## 安装dva-cli

```shell
$ npm i -g dva-cli
$ dva -v
0.7.8
```

## 创建新应用

```shell
dva new antd-quickstart
cd antd-quickstart
npm start
```

## 使用antd

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

## 创建布局

参考 [layout](https://ant.design/components/layout-cn/)





## 定义路由(pages)





## 编写UI Component

## 定义Model

## connect起来

## 后端开发

## 后端测试

## 代理

## 构建应用

