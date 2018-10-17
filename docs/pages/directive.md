## 内置指令

### 条件
- v-if
- v-else
- v-else-if

```html
 <div v-if='num==1'>1</div>
 <div v-else-if='num==2'>2</div>
 <div v-else>nana</div>
```
上述代码中有且只有一个被渲染

- v-show

```html
<div v-show='num==1'></div>
<div v-show='num==2'></div>
```
上述代码中都会被渲染出来，只是展现出来的样式不同

### 循环 v-for

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

### 数据绑定 v-bind

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

#### 样式

- 对象语法

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

- 数组语法



### html输出 v-html




### 表单输入绑定 v-model

### 事件 v-on


## 自定义指令
