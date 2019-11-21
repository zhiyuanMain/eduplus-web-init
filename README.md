# eduplus-web-init
eduplus-web

# 关于javascript

### 9012年了，就不要使用ES6之前的版本了，本项目babel预编译支持ES6+版本的使用

# 关于命名规范
### 1. *.js和js变量名采用语义化的驼峰命名法（Camel-Case），原则上不超过三个单词，杜绝Chinglish和拼音！
|说明|举例|why|
|---------------|:--------------:|--------------|
|Recommend|currentValue, exactPath|符合语义化|
|Forbidden|nowValue|Chinglish|
|Forbidden|yonghuming|拼音|

### 3. *.vue文件采用帕斯卡命名法（Pascal），但是为了遵守vue的开发规则，在`<template />`中需要自行转变为短横线命名

BreadcrumbItem.vue
```c
...
```
Breadcrumb.vue
```c   
<template>
    <div>
        <breadcrumb-item />
    </div>
</template>
<script>
    import BreadcrumbItem from './BreadcrumbItem'

    export default {
        components: {
            BreadcrumbItem
        },
    }
</script>
```

### 2. css命名规范，遵守BEM规则，具体参照[BEM规范入门](https://www.jianshu.com/p/5e018c7f0bc6)，Block层级尽量不要超过三层

html
```c
<div class="article">
    <h1 class="article__title">Title</h1>
    <section class="article__content">
        <h2 class="article__content__title">Content Title</h2>
        <span class="article__content__btn">Btn Default</span>
        <span class="article__content__btn article__content__btn-danger">Btn Danger</span>
        <span class="article__content__btn article__content__btn-info">Btn Info</span>
        // 如果这里有个列表，请再自行封装一个列表的组件
        <content-list></content-list>
    </section>
</div>
```
sass
```c
.article {
    ...

    &__title {

    }

    &__content {

        &__title {}
        &__btn {

            &-active {

            }
            &-danger {

            }
        }
    }
}

```
### 3. TO DO

# 关于eslint
使用了严格的eslint规则，在git precommit时会对代码风格进行检查 [eslint文档](https://eslint.bootcss.com/docs/rules/)

### 1. TO DO 上传前端开发规范文档

# 关于components
1.项目级components，你应该在src/components目录下新建你的组件目录, 然后在src的入口文件index.js将你的组件export出去，这样是为了避免引用组件的地方出现多个`import A from '@/componetns/`

```c
├── src
│   ├── components
│   │   ├── Breadcrumb
│   │   │   └── index.vue
│   │   ├── Hamburger
│   │   │   └── index.vue
│   │   ├── SvgIcon
│   │   │   └── index.vue
│   │   └── index.js
```

入口文件export你的组件
src/components/index.js
```c
export { default as Breadcrumb } from './Breadcrumb'
export { default as Hamburger } from './Hamburger'
export { default as SvgIcon } from './SvgIcon'
```

Parent.vue调用组件

`推荐使用：` 
```c
import { Breadcrumb, Hamburger } from ‘@/components'
```
`避免使用`
```c
import Breadcrumb from ‘@/components/Breadcrumb'
import Hamburger from ‘@/components/Hamburger'
```
### 2. TO DO

# 关于业务页面views的创建规则（需要讨论实际业务以便调整架构）
### TODO

# 关于store（vuex）的使用[https://vuex.vuejs.org/zh/installation.html](https://vuex.vuejs.org/zh/installation.html)

### 1. localStorage sessionStorage与state的关系，保证`数据源的CRUD只在一个地方被操作`,目的：项目的根数据源只在内存中存在一份
### 2. module分层
### 3. mapGetters的使用
### 4. 同步（Mutation）、异步（Action）的界限问题
### 5. TODO

# 关于axios的使用

### 1. 请求前后统一拦截器
### 2. 请求方式 GET POST PUT DELETE
### 3. 请求格式 QueryString、Request Payload、FormData
### 4. http request uri的存放与维护
### 5. TODO


# 关于Utils模块

### 1. 通用函数
### 2. 构造函数
### 3. HOC
### 4. TODO

# 关于storybook，为了遵循DRY规则
### 1. 通用组件的demo展示
### 2. note、knobs、markdown的补充
### 3. TODO

# others 
### TODO