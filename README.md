# webpack结合Vue

注意，本文默认你已经配置好了webpack，webpack配置参考：
https://github.com/ChakNS/Webpack-configuration.git

最终项目架构：

```
 vue-webpack-demo
 
+ |- /src
+   |- /components
+     |- login.vue
+     |- register.vue
+   |- main.js
+   |- index.html
+   |- router.js
+   |- style.scss
+   |- app.vue
+ |- package.json
+ |- webpack.config.js
```
## 1.安装并导入Vue包

```
cnpm i vue -S 
```
在main.js中导入Vue包

```
import Vue from 'vue';
```

注意：这里导入的vue是runtime-only的版本，功能没有vue.js的全，但vue.js版本相比仅包含runtime的版本体积要大，而且运行时的compiler转换会消耗性能，compiler过程其实可以放到构建过程中进行。因此，如果可以，请尽量用runtime-only的版本。

runtime-only在实现Vue的写法上有所不同。

## 2.创建Vue实例

```
var vm = new Vue({
    el: '#app'
})
```

## 3.安装vue-loader模块

```
cnpm i vue-loader vue-template-compiler -D
```

在webpack.config.js中配置：

```
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module: {
        rules: [
            { test: /\.vue$/, use: 'vue-loader' }
        ]
    },
plugins: [
        new VueLoaderPlugin()
    ]

```

## 4.创建组件

在src目录下创建app.vue，其内容如下：

```
<!-- 编写HTML代码 -->
<template>
  <div>
    <h1>app组件</h1>
    <p>{{ msg }}</p>
  </div>
</template>

<!-- 编写js代码 -->
<script>

<!-- export default是ES6的语法，用于暴露属性，使其可以在template中被调用，注意数据data在这里是一个方法 -->

export default {
  data() {
    return {
      msg: "123"
    };
  }
};
</script>

<!-- 编写css代码,可以按照需求使用CSS、SCSS、LESS格式，使用lang来声明，scoped表示样式仅应用于当前组件 -->
<style lang="scss" scoped>
</style>
```

## 5.在main.js中引入组件并声明

```
import app from './app.vue';

var vm = new Vue({
    el: '#app',   //
    render: c => c(app)
})
```

此时index.html中id为app的标签将会被渲染为组件内容

```
<body>
    <div id="app">
        
    </div>
</body>
```

## 6.安装并导入vue-router模块

```
cnpm i vue-router -S
```

在main.js中导入并与Vue关联：

```
import VueRouter from 'vue-router';

Vue.use(VueRouter);
```

## 7.配置路由

在src目录下创建router.js文件

```
// 引入模块
import VueRouter from 'vue-router';
// 引入组件
import Login from './components/login.vue';
import Register from './components/register.vue';

var router = new VueRouter({

    routes: [
        { path: '/login', component: Login },
        { path: '/register', component: Register }
    ]
})

// 暴露
export default router
```

在main.js中引用并声明

```
import router from './router.js'

var vm = new Vue({
    el: '#app',   //
    render: c => c(app),
    router
})
```

在app.vue中编写html代码

```
<template>
    <div>
        <router-link to='/login'>登录</router-link>
        <router-link to='/register'>注册</router-link>

        <router-view></router-view>
    </div>
</template>
```
