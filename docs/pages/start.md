## 安装

### `<script>`引入

例如

```html
<script src='[vue文件路径]'></script>
```
不使用脚手架使用示例

``` html
    <html>
        <head>
            <title>vue</title>
        </head>
        <body>
            <div id='app'>{{msg}}</div>
            <script src="http://unpkg.com/vue/dist/vue.min.js"></script>
            <script>
                var vue = new Vue({
                    el: '#app',
                    data: {
                        msg: 'hello world'
                    }
                })
            </script>
        </body>
    </html>
```

### 脚手架（vue-cli）

vue提供了一个官方的脚手架 vue-cli

- vue-cli 安装

``` bash
npm install -g vue-cli
```

vue-cli安装完成之后控制台会多一个vue命令, 如下：

![vue命令](../img/a_1.gif)

- 使用vue-cli初始化一个vue项目

```bash
vue init webpack my-project
```
![vue init](../img/a_2.gif)

生成如下

![vue project](../img/a_3.png)


## vue启程

每个vue应用都是通过Vue函数创建实例开始

```javascript
 new Vue({
     el: '#app',
     components: {component1, ...}
 })
```

例：

```js
//index.js
import Vue from 'vue'
import ComponentA from './ComponentA'

new Vue {
    el: '#app',
    data: {
        isUs: false
    },
    methods: {
        changeLaguage(){
            this.isUs = !this.isUs
        }
    },
    components: {ComponentA: ComponentA}
}
```
```html
<!--index.html-->
<html>
    <head></head>
    <body>
        <div id='#app'>
            <ComponentA :is-us='isUs'/>
        </div>
    </body>
</html>
```
> 注意当vue实例声明模板时el元素将被替换

```html
<!--ComponentA.vue-->
<template>
    <div>
        <div v-if='isUs'>{{us}}</div>
        <div v-else>{{zh}}</div>
    </div>
</template>
<script>
    export default {
        props: {
            isUs: Boolean
        },
        data() {
            return {
                us: 'hello world',
                zh: '世界你好'
            }
        }
    }
</script>
<style scoped>
</style>
```