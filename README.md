# eduplus-web-init
eduplus-web


# 关于webpack

### 1. 由于vue-cli3的集成度过高，对vue-cli版本进行了降级处理（3->2），目的是为了更加灵活地配置打包选项，且vue-cli2已经被大部分公司使用了很长时间，各种相关资料和异常处理的解决方案较多。另外一个目的可以锻炼团队成员对于webpack的应用，逐步摆脱cli，根据实际需要配置出符合业务要求的工程架构，进而形成独具特色的符合各个业务线的cm-cli。

### 2. 内置dev、test、hd、production环境变量，可以根据运行指令制定当前的运行环境
```c
    npm run dev
    npm run dev:build
    npm run tst // test指令被单元测试占用
    npm run tst:build
    npm run hd
    npm run hd:build
    npm run build
```
后期可以结合CI或者Jenkins进行在线打包，避免每次在本地打出dist包手动发布。（能交给程序做的东西，坚决不手动操作。）

### 3. 后期可以加上sentry的相关配置，做到前端的告警监听服务。

# 关于ES的命名与规范

### 9012年了，即使不用TypeScript，也不要使用ES6之前的版本了，本项目babel预编译支持ES6+版本的使用。

### 1. ES的提交规范

结合webpack、eslint、git pre-commit集成了本项目的自动化提交预检查。
使用了严格的eslint规则，在git precommit时会对代码进行检查，统一代码风格。 [eslint文档](https://eslint.bootcss.com/docs/rules/)

解决：
* debugger代码忘记删除
* unused vars过多，造成code review阅读不方便
* ide对于代码的格式化方式千奇百怪，代码结构不清晰
* 过多无用代码，如：console.log()大量存在，代码冗余过多
* 可以帮助团队成员养成良好的代码书写习惯和统一的风格

### 2. ES变量的命名规范

命名相关：
* 建议不要使用含有广告、政治相关的单词，避免引起不必要的麻烦。如：advertisement等

##### 1. *.js和js变量名采用语义化的驼峰命名法（CamelCase），原则上不超过三个单词，杜绝Chinglish和拼音！
|说明|举例|why|
|---------------|:--------------:|--------------|
|Recommend|currentValue, exactPath|符合语义化|
|Forbidden|nowValue|Chinglish|
|Forbidden|yonghuming|拼音|

##### 2. *.vue文件采用帕斯卡命名法（Pascal），但是为了遵守vue的开发规则，在`<template />`中需要自行转变为短横线命名

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

### 3. TO DO

# css规范
### 1. css属性书写顺序

建议遵循:
布局定位属性-->自身属性-->文本属性-->其他属性. 此条可根据自身习惯书写, 但尽量保证同类属性写在一起. 属性列举: 布局定位属性主要包括: margin、padding、float（包括clear）、position（相应的 top,right,bottom,left）、display、visibility、overflow等；自身属性主要包括: width & height & background & border; 文本属性主要包括：font、color、text-align、text-decoration、text-indent等；其他属性包括: list-style(列表样式)、vertical-vlign、cursor、z-index(层叠顺序) 、zoom等.
```c
.container {
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    width: 100px;
    height: 100px;
    border: 1px solid red;
    background: green;
    text-align: center;
    color: #000;
    z-inde: 1;
}
```
* 不要使用非语义化的单词，如：.w20 {width: 20px;}，可以使用hack的方式对当前元素进行重新定义
* 尽量避免.container一撸到底的情况，因为很容易造成css的覆盖。
* 尽量在每个组件内内置一个prefixCls，虽然这样做在前期的时候写<tempalte />中的内容很不爽，例如:

Breadcrumb.vue
```c
<template>
    <div :class=`${prefixCls}`>
        <header :class=`${prefixCls}__header`></header>
        <main :class=`${prefixCls}__content`></main>
        <footer :class=`${prefixCls}__footer`></footer>
    </div>
</template>
<script>
    export default {
        data() {
            prefixCls: 'cm-breadcrumb'
        }
    }
</script>
<style lang="scss" scoped>
    .cm-breadcrumb {
        &__header {

        }
        &__content {

        }
        &__footer {

        }
    }
</style>
```

`BUT！！！`当我们需要改组件的命名（产品要求，代码冲突，或者是后期觉得需要改一下命名xjj-breadcrumb），我们不需要到<tempalte />中一个一个进行修改，有的同学说可以用ied的raplaeAll的功能，但是你怕不怕js里边一堆cm-breadcrumb？我们这个时候只用修改一下
```c
export default {
    data() {
        prefixCls: 'xjj-breadcrumb'
    }
}
```
css相关的东西就搞定了！（有兴趣可以看一下ant-design的实现方式，react props和less结合的经典方案）。

### 2. css分层规范，遵守BEM规则，具体参照[BEM规范入门](https://www.jianshu.com/p/5e018c7f0bc6)，Block层级尽量不要超过三层

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

大部分Front End Engineer `痛恨`css，但是写一手优美的css又是一个FE Developer的基本功，况且`兼容ie的艰难岁月`基本上已经成为过去，我们更加需要从细节严格要求自己。



# 关于components

### 1.项目级components，你应该在src/components目录下新建你的组件目录, 然后在src的入口文件index.js将你的组件export出去，这样是为了避免引用组件的地方出现多个`import A from '@/componetns/`

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

### 2.需要把每个自定义组件都挂在vm.prototype上吗？个人觉得没有必要，因为大部分的解决方案只是将：elementUI、axios、vue-router、vuex四个挂载到原型链上，我们的自定义组件有很大一部分是`Business Components`，完全可以通过import方法引入，做到`灵活配置，有迹可循`，也可以避免vm原型链挂载对象过多导致的不可预测的问题。

# 关于业务页面views的创建规则（需要讨论实际业务以便调整架构）
### TODO

# 关于store（vuex）的使用[https://vuex.vuejs.org/zh/installation.html](https://vuex.vuejs.org/zh/installation.html)

### 1. localStorage、sessionStorage与state的关系，保证`数据源的CRUD只在一个地方被操作`,目的：项目的根数据源只在内存中存在一份，避免多处xStorage的set、get导致数据源丢失、错乱的问题。

### 2. module分层--state
原则：
1. app级：全屏、sidebar等
2. userInfo级：user相关信息、消息等
3. theme级：主题颜色
4. i18n级：多语言
5. workflow级：业务页面的流程

### 3. module分层--getters
原则：将常用的全局变量暴露在store的getters上，避免每次取值的层级较深，注意这个getters在精不在多。
```c
const getters = {
  sidebar: state => state.app.sidebar,
  device: state => state.app.device,
  token: state => state.user.token,
  avatar: state => state.user.avatar,
  name: state => state.user.name,
  roles: state => state.user.roles
}
export default getters
```
### 4. module分层--取值（mapState、mapGetters、mapMutations, mapActions, createNamespacedHelpers）
每个module进行命名空间的处理
```c
// 声明
const app = {
  namespaced: true,
  state: {
    sidebar: {
      opened: !+Cookies.get('sidebarStatus'),
      withoutAnimation: false
    },
    device: 'desktop'
  },
  ...
}
// 获取
<template>
  <el-scrollbar wrap-class="scrollbar-wrapper">
    <el-menu
      :default-active="$route.path"
      :collapse="isCollapse"
      :background-color="variables.menuBg"
      :text-color="variables.menuText"
      :active-text-color="variables.menuActiveText"
      :collapse-transition="false"
      mode="vertical"
    >
      <sidebar-item v-for="route in routes" :key="route.path" :item="route" :base-path="route.path" />
    </el-menu>
  </el-scrollbar>
</template>

<script>
import { mapGetters, createNamespacedHelpers } from 'vuex'

// createNamespacedHelpers: 不能使用mapGetters
const { mapState: mapTState } = createNamespacedHelpers('app')

import variables from '@/styles/variables.scss'
import SidebarItem from './SidebarItem'

export default {
  components: { SidebarItem },
  computed: {
    ...mapGetters(['sidebar', 'app/testGetters']),
    ...mapTState({
      testHelloWorld: state => state.device
    }),
    routes() {
      return this.$router.options.routes
    },
    variables() {
      return variables
    },
    isCollapse() {
      return !this.sidebar.opened
    }
  },
  mounted() {
    console.log(this)
  }
}
</script>

```

### 4. 同步（Mutation）、异步（Action）的界限问题
原则：所有的state写入都要使用Mutation，包括Action和业务页面的methods写入state。
### 5. 同步（Mutation）的常量设置
最好维护一个constant的文件夹，对常量进行统一管理。

# 关于axios的使用

### 1. 请求前后统一拦截器
### 2. 请求方式 GET POST PUT DELETE
### 3. 请求格式 QueryString、Request Payload、FormData
### 4. http request uri的存放与维护（src/api文件夹统一存放api配置选项）
### 5. TODO


# 关于Utils模块

### 1. 通用函数
### 2. 构造函数
### 3. HOC（mixins）
### 4. TODO

# 关于devDependencies和dependencies包的管理
原则：
1. 在满足业务需要的基础上，选用稳定、普适性高、有团队维护的npm包，如：momentjs、query-string等。
2. 如有第三方包的需要，是否需要小组讨论以避免项目过于臃肿？

# 关于storybook，为了遵循DRY规则
### 1. 通用组件的demo展示
### 2. note、knobs、markdown的补充
### 3. TODO

# 关于pull request
https://www.jianshu.com/p/2a0b3f990cd8

# 关于code review 

遵循code review的流程，会有阵痛，存在开发与deadline的冲突，但是虽然机器可以很容易阅读我们的代码，更重要的是团队其他成员可以快速理解团队当前的业务方向，大家相互吸收优秀的代码、设计思想、开发理念。`code review不是求赞、更不是批判，而是我们本着认真负责的态度为团队健康发展所做的一份贡献。`从长远看，code review坚持下去一定会提升团队和个人的水平。


# 写在最后
一套开发规范，并不是无端增加大家的工作量，而是为了团队稳定、健康、高效、持续地产出做铺垫。可能在前期的接受过程中会有痛苦、抱怨，但是当机制成熟后，我们写代码、阅读自己以前的代码、阅读别人的代码时速度都会有可观的提升，会大大节省阅读代码、培训新人的成本。真香定律永远适用在一个有纪律讲规则的团队。
