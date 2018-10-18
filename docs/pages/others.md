## 计算属性 computed

在组件中起到计算的作用

<vuep template="#example1"></vuep>

<script v-pre type="text/x-template" id="example1">
  <template>
    <div>
        <div>
            <label>firstName:</label> <input v-model='firstName'>
        </div>
        <div>
            <label>firstName:</label> <input v-model='lastName'>
        </div>
        <div>
            <label>fullName:</label> {{fullName}}
        </div>
    </div>
  </template>

  <script>
    export default {
      data: function () {
        return {
            firstName: 'zhang',
            lastName: 'san'
        }
      },
      computed: {
          fullName() {
              return this.lastName + ' ' + this.firstName
          }
      }
    }
  </script>
</script>


## 监听 watch

监听器可以监听props, computed, data内属性值的变化

```html
<template>
    <div>
        ...
    </div>
</template>
<script>
export default {
    props: {
        num: Number
    },
    data() {
        return {
            human：{firstName: '', lastName: ''}
        }
    },
    computed: {
        full() {
            ...
        }
    },
    watch: {
        num(newVal, oldVal) {
            ...
        },
        human:{
            deep: true,
            handler(n, o) {
                ...
            }
        },
        full(n, o) {

        }
    }
}
</script>
```

实例

<vuep template="#example2"></vuep>

<script v-pre type="text/x-template" id="example2">
  <template>
    <div>
        <input v-model='num'/>
        
        <div>{{count}}</div>
    </div>
  </template>

  <script>
    var interval
    export default {
        data() {
            return {
                num: 0,
                count: 0
            }
        },
        watch:{
            num(n, o) {
                var vm = this
                if(n == 0) {
                    if(interval) {
                        clearInterval(interval)
                    }
                } else {
                    interval = setInterval(function() {
                        vm.count++
                    }, 1000)
                }
            }
        }
    }
  </script>
</script>


## 过滤器 filter

过滤器可以改变目标值得显示，使用如下

```html
{{val | myFilter}}

<div :id='val | myFilter'></div>

<!--多个参数-->
{{val | myFilter(arg1, ...)}}

```

定义

```js
//全局定义
Vue.filter('myFilter', function(val [, arg1, ...]) {
    ...
})

//局部定义
{
    filters: {
        myFilter(val [, arg1, ...]) {
            ...
        }
    }
}

```

实例

<vuep template="#example3"></vuep>

<script v-pre type="text/x-template" id="example3">
  <template>
    <div>
        <input v-model='num'/>
        <div>{{num | toFixNum}}</div>
    </div>
  </template>

  <script>
    var interval
    export default {
        data() {
            return {
                num: 0
            }
        },
        filters: {
            toFixNum (val) {
                return {
                    '0': '零',
                    '1': '一',
                    '2': '二',
                    '3': '三',
                    '4': '四',
                    '5': '五'
                }[val] || '其他'
            }
        }
    }
  </script>
</script>

## 混入 mixin

混入代码结构与组件的写法基本一致，该作用主要是提高代码的复用性，混入里钩子方法先于组件内的钩子方法

```js
const myMixin = {
    data() {
        return {
            ...
        }
    },
    methods:{
        ...
    },
    created() {
        ...
    }
    ...
}

//组件内混入
...
export default {
    ...
    mixins: [myMixin]
    ...
}

//全局混入

Vue.mixin(myMixin)

```

>注意：混入内定义的属性不要与组件内的属性重复，否则会出现属性重复定义错误


```js
const myMixin = {
    created() {
        console.log('my mixin')
    }
}

new Vue({
    mixins: [myMixin],
    created() {
        console.log('my App')
    }
})


//输出  my mixin
//      my App
```
