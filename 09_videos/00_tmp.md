[TOC]

# 09 ajax to external api

```javascript
var app = new Vue({
  el: '#app',
  data: {
    startingZip: '',
    startingCity: '',
    endingZip: '',
    endingCity: '',
  },
  watch: {
    startingZip: function() {
      this.startingCity = '';
      if (this.startingCity.length == 5) {
        // call api method
        this.lookupStartingZip();
      }
    }
  },
  methods: function() {
    lookupStartingZip: _debounce(function() {
      this.startingCity = 'searching...'
    }, 500)
  }
})
```



# 08 getter & setter computed properties

```javascript
var app = new Vue({
  el: '#app',
  data: {
    first: '',
    last: ''
  },
  computed: {
    fullname: {
      get: function() {
        return this.first + ' ' + this.last
      },
      set: function(value) {
      	var name = value.split(' ');
        this.first = name[0];
        this.last = name[1];
      }
    }
  }
})
```

```html
<div id='app'>
  <input type='text' v-model='first' /> {{first}}<br />
  <input type='text' v-model='last' /> {{last}}<br />
  <input type='text' v-model='fullname' /> {{fullname}}<br />
</div>
```



# 07 computed properties

```javascript
var app = new Vue({
  el: '#app',
  data: {
    first: '',
    last: '',
    
  },
  computed: {
    fullname: function() {
      return this.first + ' ' + this.last
    }
  }
})
```

```html
<div id='app'>
  <input type='text' v-model='first' />
  <input type='text' v-model='last' />
</div>
```



# 06 event handing in vue

`v-on:click="methodToTrigger"`

`@click="methodToTrigger"`

```html
<div id='app'>
  <p>
    visit: <a :href="url">{{clearUrl}}</a>
  </p>
  <h1>{{count}}</h1>
  <button v-on:click="countUp">count up</button>
  <button @click="countDown">countDown</button>
  <input type='text' v-model='url' />
  <button @click="humanizeUrl">humanize me!</button>
</div>
```



```javascript
var app = new Vue({
  el: '#app',
  data: {
    count: 0,
    url: '',
    clearUrl: ''
  },
  methods: {
    countUp: function() {
      this.count += 1;
    },
    countDown: function() {
      this.count -= 1;
    },
    humanrizeUrl: function() {
      this.clearUrl = this.url.replace(/^https?:\/\//, '').replace(/\//, '');
    }
  }
})
```





# 05 2-way binding in vue with v-model

```vue
<template>
  <div id="app">
  	<h1>{{message}}</h1>    
    <input type='text' v-model='message' />
  </div>
</template>
<script>
	var app = new Vue({
      el: '#app',
      data: {
        message: 'hello test'
      }
	})
</script>
```



# 04 loopping with v-for directive

```vue
<template>
  <div id='app'>
      
      <ul>
        <li v-for="todo in todos">{{ todo.id }} {{ todo.text}}</li>
    </ul>
  </div>
</template>
<script>
	var app = new Vue({
      el: '#app',
      data: {
        todos: [
          {text: 'v1', id: 1},
          {text: 'v2', id: 2},
          {text: 'v3', id: 3}
        ]
      }
	})
</script>
```



# 03 using v-bind directive

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'hello vue',
    title: 'you loaded the page' + new Date(),
    url: 'http://vuejs.org/images/logo.png'
  }
})

<img v-bind:src="url" v-bind:alt="title" v-bind:title="title" />
<img :src="url" :alt="title" :title="title" />
```



# 02 directives in vue.js 

```ini
v-if
v-show # 在控制台可见节点，只是被隐藏
v-else
v-html
v-text # {{message}}
v-pre # not render
v-once # only render one time
v-cloak 
```



# 01 vue.js project setup [vue.js 2.0 fundamentals]

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
        crossorigin="anonymous">
</head>
<body>
    <div id="app">
        <h1>{{message}}</h1>
    </div>
</body>

<script src="https://unpkg.com/vue@2.3.4"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'hello vue world'
        }
    })
</script>
</html>
```

```javascript
// chrome dev console
app.message = 'hello world'
```

