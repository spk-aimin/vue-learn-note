## 内置指令

- 条件
1.v-if
2.v-else
3.v-else-if

```html
 <div v-if='num==1'>1</div>
 <div v-else-if='num==2'>2</div>
 <div v-else>nana</div>
```
上述代码中有且只有一个被渲染

4.v-show

```html
<div v-show='num==1'></div>
<div v-show='num==2'></div>
```
上述代码中都会被渲染出来，只是展现出来的样式不同

- 循环 v-for

```html
<div v-for='(item, index) in list' :key='index'>
...
</div>
```
使用 v-for时，建议指定唯一的key,而且v-for要使用在实体的dom上

```html
<template v-for='item in list'>
...
</template>
```
上述为非规范写法

- 数据绑定 v-bind

```html
<div v-bind:name='name'></div>

<!--可简写成以下形式-->
<div :name='name'></div>

<script>
export default{
    data() {
        return {
            name: 'en'
        }
    }
}
</script>
```
```html
<!--输出-->
<div name='en'></div>
<div name='en'></div>
```

- 样式

1.对象语法

```html
 <div :class='sClass' :style='sStyle'>
 </div>
 <script>
    export default {
        data() {
            return {
                sClass: {
                    'class-a': false,
                    'class-b': true
                },
                sStyle: {
                    fontSize: '12px',
                    color: 'red'
                }
            }
        }
    }
 </script>
```
```html
<!--结果-->
<div class='class-b' style='font-size: 12px; color: red'>
```

2.数组语法

- html输出 v-html

- 表单输入绑定 v-model

- 事件 v-on

- 代码演示

<vuep template="#example"></vuep>
<script v-pre type="text/x-template" id="example">
  <template>
    <div>
        <div>Hello, {{ name }}!</div>
        <div>
            <button type='button' v-on:click='changeName'>change name</button>
        </div>
        <!--条件-->
        <div v-if='num1==1'>
             num1 = 1
        </div>
        <div v-if='num2==1'>
            num2==1
        </div>
        <div v-else>num ....</div>
        
        <!--列表-->
        <div v-for='(item, key) in obj' :key='key'>
            {{key}} : {{item}}
        </div>

        <div v-for='(item, index) in list' :key='index'>
            {{item.name}}
        </div>
        <!--v-model-->
        <div>
            <input v-model='name'/>
        </div>
        <!-- v-hmtl -->
        <div>{{txt}}</div>
        <div v-html='txt'></div>
    </div>
  </template>

  <script>
    export default {
        data() {
            return {
                name: 'wolrd',
                num1: 1,
                num2: 2,
                obj: {name: '张三', gender: '男'},
                list: [{name: '张三'}, {name: '李四'}],
                txt: '<p style="color: red">张三你好</p>',
                sClass: {
                    'class-a': false,
                    'class-b': true
                },
                sStyle: {
                    fontSize: '12px',
                    color: 'red'
                }
            }
        },
        methods: {
            changeName(e) {
                console.log(e)
                this.name = 'china'
            }
        }
    }
  </script>
</script>

## 自定义指令

除了vue内置指令，开发者可以自定义指令，语法如下

```js
//全局
vue.directive('hover', {...})

//局部

{
    directives: {
        hover: {
            ...
        }
    }
}
```
使用

```html
<div v-hover>test</div>
```

钩子函数

>bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

>inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

>update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可>能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

>componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。

>unbind：只调用一次，指令与元素解绑时调用。

钩子函数参数

>el：指令所绑定的元素，可以用来直接操作 DOM 。

>binding：一个对象，包含以下属性：
> >name：指令名，不包括 v- 前缀。
 
> >value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。

> >oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。

> >expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。

> >arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。

> >modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。

>vnode：Vue 编译生成的虚拟节点。移步 VNode API 来了解更多详情。

>oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

```html
 <template>
    <div v-hover='name'>Hello, {{ name }}!</div>
  </template>

  <script>
    export default {
      data: function () {
        return { name: 'Vue' }
      },
      directives: {
          hover: {
            bind(el, binding) {
            },
            inserted(el, binding) {
                el.addEventListener('mouseover', function(e) {
                    el.style.color = 'red'
                    console.log(binding.value)
                }, false)
                el.addEventListener('mouseout', function(e) {
                    el.style.color = ''
                }, false)
            },
            update(el, binding) {
            },
            unbind(el, binding) {
            }
          }
      }
    }
</script>
```