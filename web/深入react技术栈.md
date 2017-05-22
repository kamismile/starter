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

