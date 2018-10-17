## 组件定义

组件是可以复用的vue实例, 组件都有一个名字，这个名字可以以标签的形式运用在其他组件中， 例如

```js
Vue.component('component-a', {...})//组件A

vue.component('component-b', {//组件B
    template: '<div><component-a></componnet-a></div>'
})
```
一个vue应用是由很多组件组件，其结构如如同一个树形结构

![component tree](../img/a_5.png)

## 组件注册 

- 全局注册
  
一个组件由Vue的component方法声明，当该组件被声明后，在其他组件中无需声明该组件可直接使用

```html
<!--ComponentA.vue-->
<template>
    <div>
        ...
    </div>
</template>
<script>
    export default {
        data() {
            return {}
        }
        ...
    }
</script>
```

```js
//index.js
import Vue from 'vue'
import ComponentA from './ComponentA'

Vue.component('component-a', ComponentA)
```

```html
<!--ComponentB.vue-->
<template>
    <div>
        ...
        <component-a></component-a>
        ...
    </div>
</template>
<script>
    export default {
        data() {
            return {...}
        }
        ...
    }
</script>
```

- 局部注册

在组件中，定义在components属性中

```html
<!--ComponentA.vue-->
<template>
    <div>
        ...
    </div>
</template>
<script>
    export default {
        data() {
            return {...}
        }
        ...
    }
</script>
```

```html
<!--ComponentB.vue-->
<template>
    <div>
        ...
        <component-a></component-a>
        ...
    </div>
</template>
<script>
    import ComponentA from './Component'
    export default {
        components: {ComponentA: ComponenetA},
        data() {
            return {
                ...
            }
        }
        ...
    }
</script>
```
> 组件中data必须是一个函数

<vuep template="#example"></vuep>

<script v-pre type="text/x-template" id="example">
  <template>
    <div>
        <component-a></component-a>
        <component-a></component-a>
    </div>
  </template>

  <script>
    var ComponentA = {
        data: function() {
            return {
                count: 0
            }
        },
        // data: {
        //     count: 0
        // },
        methods: {
            countFn: function(){
                this.count++
            }
        },
        template: '<button @click="countFn()">You clicked me {{ count }} times.</button>'
    }
    export default {
      data: function () {
        return { name: 'Vue' }
      },
      components: {ComponentA}
    }
  </script>
</script>

## 组件间的通信

### 父 ——> 子

- prop

子组件是以标签的形式体现在父组件中，父组件可以将值赋予子组件标签属性中， 子组件props属性接收父组件传递过来的值

```html
<!--父组件-->
<template>
    <div>
        <child v-bind:info='msg'  v-bind:num='2' v-bind:bool='false' str='zs'></child>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                msg: 'hello world'
            }
        }
    }
</script>
```

```html
<!--child-->
<template>
    <div></div>
</template>
<script>
    export default {
        props: {
            info: String,  //hello world
            num: Number,   // 2
            bool: Boolean, // false
            str: String  //zs
        },
        data() {
            msg: 'hello'
        },
        created() {
            console.log(this.info)
            this.info = 'zhangsan'// 报错
            this.msg = 'world' 
        }
    }
</script>
```

> props属性内定义的属性值为只读属性，在子组件内不可改变其值或者其引用。
> props内声明的属性需要指定类型

```js
{
    props: {
        a1: Number,//只声明类型
        a2: {
            type: Number
        },
        a3: {//属于多种类型
            type: [Number, String]
        },
        a4: {//赋予默认值
            type: Number,
            default: 2
        },
        a5: {
            type: Object,
            default() {
                return {}
            }
        }
    }
}
```

vue支持的校验类型有以下几种

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

### 子 ——> 父： 自定义事件

vue提供了emit方法, on方法与v-on指令实现了自定义事件的定义与触发

```js
//子组件
this.$emit('myEvent', data)

//父组件
<child v-on:myEvent='callback'></child>
```


## vue内置组件

### 插槽 slot

插槽的作用是将组件或者其他元素插入到某组件的slot存在的位置，例如

```html
<!--组件A-->
...
<div>
    <slot></slot>
</div>
...
```
```html
<!--组件B-->
....
<component-a>
    <p>hello</p>
</component-a>
....
```
```html
<!--输出-->
...
<div>
    <p>hello</p>
</div>
...

```
slot拥有一个name属性，可以使组件拥有多个插槽

```html
...
<div class='A'>
    <slot name='A'></slot>
</div>
<div class='D'>
    <slot></slot>
</div>
<div class='B'>
    <slot name='B'></slot>
</div>
...
```

```html
<!--组件B-->
....
<component-a>
    <p slot='A'>hello</p>
    <p>world</p>
    <p slot='B'>张三</p>
</component-a>
....
```
```html
<!--输出-->
...
<div class="A">
    <p>hello</p>
</div>
<div class="D">
    <p>world</p>
</div>
<div class="B">
    <p>张三</p>
</div>
...

```


### 动态组件component

使用如下

```html
<component :is='value'></component>
```
其中is属性为必选项，确定使用哪个组件， is取值为已注册的组件名或者组件对象； 其他属性与自定义组件相符


### 动画 transition