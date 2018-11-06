---
title: Vue.js的基本使用
date: 2018-04-20 17:11:46
bgimage: "/images/vuejs/bg.png"
tags:
- vue.js
categories:
- Vue.js的基本使用
---
## 路由的使用

1. 安装路由模块

```
Npm install vue-router --save
```

2. 引入模块

```
import VueRouter from ‘vue-router’
```

3. 作为vue的插件

```
Vue.use(VueRouter)
```

4. 创建路由实例对象

```
New VueRouter({
...配置参数
})
```

5. 注入vue选项参数

```
New Vue({
router
})
```

6. 告诉路由渲染的位置

```
<router-view></router-view>
```
## Hash 和 History 模式

vue-router默认 hash 模式 "#" url的hash模式，mode:history模式就是正常的路径模式,history带来的便利是可以使用浏览器的前进后退功能

```
export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/home',
      name: 'Home',
      component: Home
    },
    {
      path: '/project',
      name: 'Project',
      component: Project
    },
    {
      path: '/doc',
      name: 'Document',
      component: Document
    }

  ]
})
```

## router-link的各项配置

router-link 组件支持用户在具有路由功能的应用中（点击）导航。 通过 to 属性指定目标地址，默认渲染成带有正确链接的 a 标签，可以通过配置 tag 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名

```
<router-link :to="{path:'/project'}" active-class="activeClass" tag='li' event='mouseover'>
  <i class="fa fa-home"></i>
  <span>project</span>
</router-link>
```
to：目标路由的链接，此链接可以进行动态绑定的，tag：渲染成某种标签，如:li,event:默认为点击事件，也可以将其设置为鼠标移入的事件，mouseover

```
<router-link :to='index' tag='li' event='mouseover'>
  <i class="fa fa-home"></i>
  <span>Home</span>
</router-link>


<script>
export default {
  name: 'App',
  data () {
    return {
      index: '/home'
    }
  }
}
</script>
```

active-class：设置 链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 linkActiveClass 来全局配置

```
//路由中设置

export default new Router({
  mode: 'history',
  linkActiveClass: 'is-active',
  routes: [
    {
      path: '/home',
      name: 'Home',
      component: Home,
      // 路由中的别名
      alias: '/index'
    },
    {
      path: '/project',
      name: 'Project',
      component: Project
    },
    {
      path: '/doc',
      name: 'Document',
      component: Document
    }

  ]
})

//组件中进行设置

<router-link :to="{path:'/project'}" active-class="activeClass" tag='li' event='mouseover'>
    <i class="fa fa-home"></i>
    <span>project</span>
</router-link>
```

## 重定向和别名

```
export default new Router({
  mode: 'history',
  linkActiveClass: 'is-active',
  routes: [
    {
      path: '/home',
      name: 'Home',
      component: Home,
      // 路由中的别名
      alias: '/index'
    },
    {
      path: '/project',
      name: 'Project',
      component: Project
    },
    {
      path: '/doc',
      name: 'Document',
      component: Document
    },
    // 如果没有以上的地址，就将跳转到home页面
    {
      path: '*',
      // component: Home
      // redirect: '/home'
      // redirect:{path: '/home'}
      // redirect:{name: 'Home'}
      // 动态设置重定向的目标路径
      redirect: (to) => {
        // 目标路由对象，就是访问的路径的路由信息
        if (to.path === '/123') {
          return '/home'
        } else if (to.path === '/456') {
          return {path: '/doc'}
        } else {
          return {name: 'Project'}
        }
        // return '/home'
      }
    }

  ]
})
```

## 嵌套路由使用

exact："是否激活" 默认类名的依据是 inclusive match （全包含匹配）

```
<router-link to='/' exact tag='li' event='mouseover'>
    <i class="fa fa-home"></i>
    <span>Home</span>
</router-link>
```

子路由的配置

```
export default new Router({
  mode: 'history',
  linkActiveClass: 'is-active',
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/home',
      name: 'Home',
      component: Home,
      alias: '/index'
    },
    {
      path: '/project',
      component: Project,
      children: [
        {
          path: '',
          name: 'Project',
          // 默认子路由，有默认子路由就不要再父路由中设置name属性
          component: study
        },
        {
          path: 'work',
          name: 'work',
          component: work
        },
        {
          path: 'hobby',
          name: 'hobby',
          component: hobby
        }
      ]
    },
    {
      path: '/doc',
      name: 'Document',
      component: Document
    }
  ]
})
```

## 命名视图

子路径的格式为：http://localhost:8081/work，即在子路径中加了/，就相对于跟路径来说的，子路径不需要嵌套，但是组件需要嵌套的。。

```
//vue页面的写法，动态的绑定路径

<ul class="nav">
    <router-link :to="{name: 'Project'}" exact tag='li'>
    <a>study</a>
    </router-link>

    <router-link :to="{name: 'work'}" tag='li'>
    <a>work</a>
    </router-link>

    <router-link :to="{name: 'hobby'}" tag='li'>
    <a>hobby</a>
    </router-link>
</ul>

//路由的配置

export default new Router({
  mode: 'history',
  linkActiveClass: 'is-active',
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/home',
      name: 'Home',
      component: Home,
      alias: '/index'
    },
    {
      path: '/project',
      component: Project,
      children: [
        {
          path: '',
          name: 'Project',
          component: study
        },
        {
          path: '/work',
          name: 'work',
          component: work
        },
        {
          path: '/hobby',
          name: 'hobby',
          component: hobby
        }
      ]
    },
    {
      path: '/doc',
      name: 'Document',
      component: Document
    }
  ]
})
```
多个router-view的应用

```
//app.vue

<router-view name="slider"></router-view>
<router-view class="center"></router-view>

//路由中的配置，一个路径对应一个组件用component，一个路径对应多个组件，用Components，默认的组件用default，其他的组件用router-view中name的值

export default new Router({
  mode: 'history',
  linkActiveClass: 'is-active',
  routes: [
    {
      path: '/',
      component: Home
    },
    {
      path: '/home',
      name: 'Home',
      component: Home,
      alias: '/index'
    },
    {
      path: '/project',
      component: Project,
      children: [
        {
          path: '',
          name: 'Project',
          component: study
        },
        {
          path: '/work',
          name: 'work',
          component: work
        },
        {
          path: '/hobby',
          name: 'hobby',
          component: hobby
        }
      ]
    },
    {
      path: '/doc',
      name: 'Document',
      components: {
        default: Document,
        slider: slider
      }
    }
  ]
})
```



