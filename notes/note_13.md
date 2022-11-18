## 尚硅谷Vue系列教程学习笔记（13）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)


#### 121-路由的query参数


路由传入query参数的一般写法：


```
 <li><router-link 
      :to="`/home/message/detail?id=${msg.id}&title=${msg.title}&content=${msg.content}`">消息编号：{{msg.id}}，消息标题：{{msg.title}}</router-link>&nbsp;&nbsp;
      </li>

```



路由传入参数的对象写法：

```
 <li><router-link 
      :to="{
        path:'/home/message/detail',
        query:{
          id:msg.id,
          title:msg.title,
          content:msg.content
        }
      }">消息编号：{{msg.id}}，消息标题：{{msg.title}}</router-link>&nbsp;&nbsp;
      </li>

```



#### 122-命名路由

给路由命名的作用：直接在vue组件中调用name属性即可实现组件的路由切换。



案例代码：

```
  :to="{
        path:'/home/message/detail',
        name:"detail",//直接在:to属性后边对象中添加name属性即可实现组件的定位和路由
        query:{
          id:msg.id,
          title:msg.title,
          content:msg.content
        }
      }">消息编号：{{msg.id}}，消息标题：{{msg.title}}</router-link>&nbsp;&nbsp;

```


#### 123-路由中的params参数


使用params传入参数的案例代码：


```
//在父组件中写入如下代码
<!--使用params传入参数的方法-->
      <li><router-link 
      :to="`/home/message/detail/${msg.id}/${msg.title}/${msg.content}`">消息编号：{{msg.id}}，消息标题：{{msg.title}}</router-link>&nbsp;&nbsp;
      </li>


//在route/index.js文件中写入如下代码
{
    path: "detail/:id/:title/:content",
    name: "detail",
    component: MessageDetail
}


//在路由的下一级组件中（子组件）写入如下代码
<template>
<h2>消息编号：{{$route.params.id}}</h2>
<h3>消息标题：{{$route.params.title}}</h3>
<h3>消息内容：{{$route.params.content}}</h3>
</template>

<script>
export default {
    name:"MessageDetail"
}
</script>

<style>
...
</style>

```

上述使用params传入参数的对象写法代码：

```
 <!--使用params传入参数的对象写法-->、
      <li>
        <router-link
          :to="{
            path: '/home/message/detail',
            params: {
              id: mag.id,
              title: msg.title,
              content: msg.content,
            },
          }"
          >消息编号：{{ msg.id }}，消息标题：{{ msg.title }}</router-link
        >&nbsp;&nbsp;
      </li>

```



#### 124-路由的props配置


props配置的第1种写法，即值为对象的写法：

```
//在route/index.js文件中的代码
 {
    path: "detail/:id/:title/:content",
    name: "detail",
    component: MessageDetail,
     //设置props属性接受传来的值
    props: {
        id: 1,
        title: '测试',
        content: '内容'
    }   
}

```

但是上述写法用的比较少，第2种写法如下：

```


//在route/index.js文件中代码
{
    path: "detail/:id/:title/:content",
        name: "detail",
    component: MessageDetail,
    //设置props属性接受传来的值，第一种一般的props写法
    // props: {
    //     id: 1,
    //     title: '测试',
    //     content: '内容'

                            //第二种props写法
    props:true//表示该路由组件接受到的params参数以props的形式传递给该组件，在该组件中使用props属性接受即可
}



//在接收组件中的代码，使用props属性接收
<template>
<h2>消息编号：{{id}}</h2>
<h3>消息标题：{{title}}</h3>
<h3>消息内容：{{content}}</h3>
</template>

<script>
export default {
    name:"MessageDetail",
    //在接收组件中使用props属性获得传来的值
    props:['id','title','content']
}
</script>

<style>

</style>


```


第3种路由组件使用props属性的写法：
```
//在route/index.js文件中的代码为
{
    path: "detail/:id/:title/:content",
    name: "detail",
    component: MessageDetail,

    //设置props属性接受传来的值，第1种一般的props写法
    // props: {
    //     id: 1,
    //     title: '测试',
    //     content: '内容'
    
    // //第2种props写法
    // props: true//表示该路由组件接受到的params参数以props的形式传递给该组件，在该组件中使用props

    //第3种写法，使用props函数形式返回值
    props($route) {
        return {
            id: $route.params.id,
            title: $route.params.title,
            content: $route.params.content
        }
    }

}

```



### 125-router-link的replace属性


注意：浏览器中可以记录用户点击页面的出现顺序，然后点击back或前进按钮可以切换到历史页面，这种记录的形式称作push模式。


//在router-link标签中加入replace属性之后，点击页面时，浏览器不再记录历史页面
```
<router-link replace class="list-group-item" active-class="active" to="/about">About</router-link>
          <router-link replace class="list-group-item" active-class="active" to="/home">Home</router-link>
```

解释：replace属性的作用是替换掉栈顶的页面。



#### 126-编程式路由导航

也就是不借助router-link属性实现路由导航的方式，叫做编程式路由导航。

实现案例代码：


```
//父组件绑定点击事件，并在其中使用push或replce方式写入代码

  methods: {
    pushShow(msg) {
      console.log("使用push方式实现页面跳转...")
      this.$router.push({
        //使用push的方法实现路由组件的跳转
        path: "/home/message/detail",
        query: {
          id: msg.id,
          title: msg.title,
          content: msg.content,
        }
      });
    },

    replaceShow(msg) {
      console.log("使用replace方式实现页面跳转...")
      this.$router.replace({
        path:'/home/message/detail',
        query:{
           id: msg.id,
          title: msg.title,
          content: msg.content,
        }
      })

    },
  },


```



#### 127-缓存路由组件

Vue2中使用keep-alive：
```
 <!--vue2中使用keep-alive-->
      <!--添加keep-alive属性，当前组件被切换之后，不会被销毁-->
      <keep-alive include="News">
        <router-view></router-view>
      </keep-alive>
```



Vue3中使用keep-alive：
```
<!--Vue3中使用keep-alive-->
      <router-view v-slot="{ Component }">
        <!-- 所有页面缓存 -->
        <keep-alive include="News">
          <component :is="Component" />
        </keep-alive>
      </router-view>
```


上述使用keep-alive时，如果需要对多个组件进行缓存，那么需要如下写法：
```

 <!--Vue3中使用keep-alive-->
      <router-view v-slot="{ Component }">
        <!-- 所有页面缓存 -->
        <keep-alive :include="['News','xxx','xxx']">
          <component :is="Component" />
        </keep-alive>
      </router-view>

```


#### 128-两个新的生命周期钩子


路由组件独有的两个生命周期钩子为：
```

 //使用路由组件独有的生命周期钩子
  //被激活时调用
  activated(){
    console.log("News组件被激活了...")
     this.timer = setInterval(()=>{
      console.log("激活时定时器被调用...")
      this.opacity -= 0.1
      if(this.opacity <= 0){
        this.opacity = 1
      }
    }, 16)
  },
  //未被激活时调用
  deactivated(){
    console.log("News未被激活...")
    clearInterval(this.timer)
  }

```


#### 129-全局前置-路由守卫

重要性：路由守卫很重要，开发过程中用的很多。


作用：给点击路由切换组件设置权限。


设置权限的位置：router路由器里面设置缺陷判断逻辑是最合适的。

权限判断时机：在切换组件之前进行判断。

路由守卫被调用时机：初始化组件时调用、每次切换组件之前调用。


路由守卫实现案例代码：

```
//在route/index.js的代码
//引入vue-router插件
import { createRouter, createWebHashHistory } from 'vue-router'   // 使用插件可以不加这个

//引入组件
import About from '../pages/About.vue'
import Home from '../pages/Home.vue'
import Message from '../pages/Message.vue'
import News from '../pages/News.vue'
import MessageDetail from '../pages/MessageDetail.vue'


//配置路径：key:value的对应关系
const routes = [...]

//创建路由器对象
const router = createRouter({
    history: createWebHashHistory('/'),  // 编写后台就这么写
    routes,
})


//在暴露出去之前添加一个路由守卫
router.beforeEach((to, from, next) => {
    console.log("路由守卫被调用...", to, from)
    //增加判断逻辑
    if (to.path === '/home/news' || to.path === '/home/message') {
        alert("对不起，您当前无权限！")
    } else {
        next()
    }

    //调用next函数，表示处理过上述逻辑之后，继续往下走
    // next()
})


export default router

```



#### 130-全局后置-路由守卫

重点：mets属性中可以放入一些自己定义的变量信息，叫做路由元信息。


后置路由守卫对应的回调函数中是没有next参数的，只有to和from参数。

需求：在切换组件之后，在网页上的title随之发生切换。

后置路由守卫实现代码：

```
//在route/index.js中的代码

//后置路由守卫设置，其中只有to，from两个参数
router.afterEach((to, from) => {
    console.log('后置路由守卫...', to, from)
    document.title = to.meta.title || '端云一体的缺陷检测系统'//切换组件之后，对组件展示的title进行修改
})


//注意需要在每个routes关系中设置meta属性，并向其中添加title属性。

```

//截止到2022.9.2晚上18：50止













