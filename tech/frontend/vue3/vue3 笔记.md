## vite 基本使用
创建项目
```shell
npm init vite-app 项目名称
或
yarn create vite-app 项目名称
```

安装依赖
```shell
npm i 
或 
yarn
```
启动项目
```shell
npm run dev
或
yarn dev
```


## 创建vue应用
基本步骤：
-   在 main.js 中导入 `createApp` 函数
-   定义 App.vue 组件，导入 main.js
-   使用 `createApp` 函数基于 App.vue 组件创建应用实例
-   挂载至 index.html 的 \#app 容器

**App.vue**

```html
<template>
  <div class="container">
    我是根组件
  </div>
</template>
<script>
export default {
  name: 'App'
}
</script>
```
**main.js**
```js
import {createApp} from 'vue'
import App from './App.vue'
const app = createApp(App)
app.mount('#app')
```

## 选项API和组合API
什么是选项API 写法：`Options ApI`
-   在 vue2.x 项目中使用的就是 `选项API` 写法
    -   代码风格：data选项写数据，methods选项写函数...，一个功能逻辑的代码分散。
-   优点：易于学习和使用，写代码的位置已经约定好
-   缺点：代码组织性差，相似的逻辑代码不便于复用，逻辑复杂代码多了不好阅读。

什么是组合API写法：`Compositon API`
-   咱们在vue3.0项目中将会使用 `组合API` 写法
    -   代码风格：一个功能逻辑的代码组织在一起（包含数据，函数...）
-   优点：功能逻辑复杂繁多情况下，各个功能逻辑代码组织再一起，便于阅读和维护
-   缺点：需要有良好的代码组织能力和拆分逻辑能力

> 为了能让大家较好的过渡到 vue3.0 的版本来，`也支持 vue2.x 选项API写法`

## 组合API-setup函数
-   `setup` 是一个新的组件选项，作为组件中使用组合API的起点。
-  从组件生命周期来看，它的执行在组件实例创建之前 `vue2.x的beforeCreate` 执行。
-   这就意味着在 `setup` 函数中 `this` 还不是组件实例，`this` 此时是 `undefined`
-   在模版中需要使用的数据和函数，需要在 `setup` 返回。

## 组合API-生命周期
| 生命周期函数    | 作用时期   |
| --------------- | ---------- |
| setup           | 创建实例前 |
| onBeforeMount   | 挂载DOM前  |
| onMounted       | 挂载DOM后  |
| onBeforeUpdate  | 更新组件前 |
| onUpdated       | 更新组件后 |
| onBeforeUnmount | 卸载销毁前 |
| onUnmounted     | 卸载销毁后 |

> 总结： 组合API的生命周期钩子有7个，可以多次使用同一个钩子，执行顺序和书写顺序相同。

