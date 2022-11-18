## 尚硅谷Vue系列教程学习笔记（14）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)




#### 131-独享路由守卫

给每个组件添加路由守卫的案例代码：

```
//在store/index.js中的代码
//配置路径：key:value的对应关系
const routes = [
    {
        path: '/about',
        name: "about",
        component: About,
        meta: {
            title: "关于"
        }
    },
    {
        name: "home",
        path: '/home',
        component: Home,
        meta: {
            title: "主页"
        },
        children: [
            {
                name: "message",
                path: "message",
                component: Message,
                meta: {//设置需要权限校验属性
                    isAuth: true,
                    title: "消息列表"
                },
                //设置独享路由守卫，由一个组件独享
                beforeEnter: (to, from, next) => {
                    console.log("前置路由守卫被调用...", to, from)
                    //实现网页title随着组件切换改变，需要在上述的每个路由中添加路由元数据meta中进行添加

                    //增加判断逻辑
                    if (to.meta.isAuth) {//判断是否需要权限校验
                        alert("对不起，您当前无权限！")
                    } else {
                        next()
                    }
                    //调用next函数，表示处理过上述逻辑之后，继续往下走
                    // next()
                },
                children: [
                    {
                        name: "detail",
                        path: "detail",
                        component: MessageDetail,
                        meta: {
                            isAuth: true,
                            title: "消息详情"
                        }
                        //设置props属性接受传来的值，第一种一般的props写法
                        // props: {
                        //     id: 1,
                        //     title: '测试',
                        //     content: '内容'
                        // }

                        // //第二种props写法
                        // props: true//表示该路由组件接受到的params参数以props的形式传递给该组件，在该组件中使用props属性接受即可

                        // props($route) {
                        //     return {
                        //         id: $route.params.id,
                        //         title: $route.params.title,
                        //         content: $route.params.content
                        //     }
                        // }
                    }
                ]
            },
            {
                name: "news",
                path: "news",
                component: News,
                meta: {//设置需要权限校验属性
                    isAuth: true,
                    title: "新闻列表"
                },
                beforeEnter: (to, from, next) => {
                    console.log("前置路由守卫被调用...", to, from)
                    //实现网页title随着组件切换改变，需要在上述的每个路由中添加路由元数据meta中进行添加

                    //增加判断逻辑
                    if (to.meta.isAuth) {//判断是否需要权限校验
                        alert("对不起，您当前无权限！")
                    } else {
                        next()
                    }
                    //调用next函数，表示处理过上述逻辑之后，继续往下走
                    // next()
                },
            }
        ]
    },
]


```


重点：独享路由守卫只有前置没有后置。



#### 132-组件内路由守卫

作用：如果该组件有独有的逻辑需要在进入或离开时进行处理，那么就可以使用组件内路由守卫。


案例实现代码：

```
<template>
  <h2>我是About的内容</h2>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "About",
  //通过路由规则进入该组件时被调用
  beforeRouteEnter(to, from, next){
    console.log("About组件进入时调用...")
    next()
  },

  //通过路由规则离开该组件时被调用
  beforeRouteLeave (to, from, next) {
    console.log("About组件离开时调用...")
    next()
  }
};
</script>

<style>
</style>

```



#### 133-history模式和hash模式

路由器的两种工作模式：history模式和hash模式。

重点：路径里面从#开始之后的所有值都算作为路由中的hash值，所有的hash值不会随着请求发送给服务器的。

两种模式下路径的区别：
使用hash模式之后，路径中会出现：/#/
使用history之后，路径中很干净只有：/


项目上线之前需要对编写的页面进行打包，之后才可以上线部署运行。


重点：Vue3项目中使用route/index.js中添加属性mode更改路由器默认工作模式的示例代码如下：

```

//创建路由器对象
const router = createRouter({
    //配置路由模式为hash模式
    history: createWebHashHistory(),
    //配置路由模式为history模式
    history: createWebHistory(),
    routes
})


```

重点：使用build指令对编写好的项目进行打包案例代码的步骤：
1.在命令行运行：npm run build即可，执行时间取决于项目大小（注意：Vue只看src文件夹下有什么，就打包什么，打包出来的文件需要在服务器上进行部署才可以展现）；
2.然后在项目中出现了一个dist文件夹；
3.项目上线之前需要创建一个微型的服务器，创建服务器的步骤如下：
    （1）在桌面创建一个demo文件夹，然后用vscode打开，然后在命令行写入：npm init，除了服务器名字自定义之外，其他的一路回车即可，完成项目初始化；
    （2）然后，在命令行里面安装express，使用命令：npm i express；
    （3）然后在demo项目中创建一个sever.js文件，其中写入代码为：
    ```
const express = require('express')

const app = express()

//配置服务器响应get请求
app.get('/person', (req, res) => {
    //返回数据配置
    res.send({
        name: 'wyy',
        age:22
    })
})


app.listen(5005, (err) => {
    if(!err) console.log("服务器启动成功...")
})

    ```

    （4）然后在命令行直接运行node server启动服务器成功；

4.然后在demo项目中创建一个static文件夹，其中存放一些静态资源文件，这里来自于使用npm run build打包的使用Vue-cli打包之后的dist文件夹里面的文件。


5.然后在server.js文件中写入如下代码，目的是为让服务器识别static文件夹下面的静态资源：
```

const app = express()

//让服务器使用静态资源
app.use(express.static(__dirname+'/static'))


```


6.同时为了解决使用路由模式为history时出现的刷新页面请求时出现的404问题，还需要在server.js中引入如下代码：
```

const history = require('connect-history-api-fallback')


//注意需要在解析static之前使用
//在使用静态资源之前使用上述的connect-history-api-fallback
app.use(history())

```


7.完成项目的简单部署。



#### 134-element-ui的基本使用

Vue中组件分为两大类：
1.移动端UI组件：Vant/Cube UI/Mint UI等；
2.PC端UI组件：element-UI/IView UI等；


注意：使用开源UI组件需要项目不那么定制化实现，如果项目定制化实现，那么不推荐使用开源的UI组件。

注意：一些典型的管理系统适合使用开源UI组件。


在项目中使用element-ui组件的案例代码，在App.vue组件中：

```

<template>
  <div>
    <!-- 日期选择器 -->
    <el-date-picker
      v-model="queryParams.begintime"
      type="daterange"
      unlink-panels
      :start-placeholder="starDate"
      :end-placeholder="endDate"
      :shortcuts="shortcuts"
      value-format="YYYY-MM-DD"
      :disabled-date="disabledDate"
      @calendar-change="handleChange"
      @focus="handleFocus"
    />
  </div>
</template>

<script>
import { ref, reactive } from "vue";
export default {
  name: "App",
  setup() {
    // 日期选择器默认开始时间-上周年月日
    let starDate = new Date();
    starDate.setTime(starDate.getTime() - 3600 * 1000 * 24 * 7);
    starDate = getNewTime(starDate);
    // 日期选择器默认结束时间-当前年月日
    let endDate = getNewTime(new Date());
    // 日期选择器快捷方式
    const shortcuts = [
      {
        text: "上周",
        value: () => {
          const end = new Date();
          const start = new Date();
          start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
          return [start, end];
        },
      },
      {
        text: "上月",
        value: () => {
          const end = new Date();
          const start = new Date();
          start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
          return [start, end];
        },
      },
      {
        text: "过去三个月",
        value: () => {
          const end = new Date();
          const start = new Date();
          start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
          return [start, end];
        },
      },
    ];
    // 查询条件
    const queryParams = reactive({
      begintime: null,
    });
    // 当前选择的开始日期
    const pickDay = ref(null);
    // 获取当前年月日的函数
    function getNewTime(date) {
      let y = date.getFullYear(); //年
      let m = date.getMonth() + 1; //月，月是从0开始的所以+1
      let d = date.getDate(); //日
      m = m < 10 ? "0" + m : m; //小于10补0
      d = d < 10 ? "0" + d : d; //小于10补0
      return y + "-" + m + "-" + d; //返回时间形式yyyy-mm-dd
    }
    // 时间选择器禁用范围
    const disabledDate = (time) => {
      if (!pickDay.value) {
        return false;
      } else {
        const con1 =
          new Date(pickDay.value).getTime() - 1 * 24 * 60 * 60 * 1000;
        const con2 =
          new Date(pickDay.value).getTime() + 30 * 24 * 60 * 60 * 1000;
        return time < con1 || time > con2;
      }
    };
    // 日期选择器选择时触发
    function handleChange(val) {
      const [pointDay] = val;
      pickDay.value = pointDay;
    }
    // 日期选择器获得焦点时触发
    function handleFocus() {
      pickDay.value = null;
    }
    return {
      starDate,
      endDate,
      shortcuts,
      queryParams,
      disabledDate,
      handleChange,
      handleFocus,
    };
  },
};
</script>

```

注意：上述直接在main.js中引入所有的css文件和组件对前端项目是非常不友好的，因为会造成很多的代码冗余，且项目体积过大。



#### 135-element-UI组件按需引入


Vue3中配置如下：
1.首先安装两个依赖：npm install -D unplugin-vue-components unplugin-auto-import；
2.然后，在项目的vue.config.js文件中写入如下代码：


```
const { defineConfig } = require('@vue/cli-service')

//按需引入element-ui组件
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')

module.exports = defineConfig({
  transpileDependencies: true,
  
  // 和webpapck属性完全一致，最后会进行合并
  configureWebpack: {
    //配置webpack自动按需引入element-plus，
    plugins: [
      AutoImport({
        resolvers: [ElementPlusResolver()]
      }),
      Components({
        resolvers: [ElementPlusResolver()]
      })
    ],
  }
})

```


3.在App.vue组件中使用案例代码如下：

```

//待定，查了很多资料，但是仍然没有找到合适的解决方案（使用js语言）
...

```


#### 136-Vue3的基本使用


重点：webpack支持Tree Shaking，来消除js中多余的没有用的代码。

Vue3中更好的与ts进行结合；

新特性：
1.组合API；
2.新的内置组件；
3.其它改变（例如：新的生命周期钩子等）；
...



#### 137-创建Vue3-CLI脚手架项目


使用如下代码判断Vue版本是否在4.5.0及以上：
```
vue --version
//或使用
vue -V

```

#### 138-使用vite创建工程


现在使用的创建前端项目的工具为webpack。

重点：vite是新一代的创建前端项目的工具。
vite的优点：
1.vite中可快速打包，且快速冷启动；
2.轻量快速的热重载；
3.真正地按需编译，不再等待整个应用编译完成；



使用vite创建Vue3应用的步骤：
1.运行：npm init vite-app vue3_test_vite；
2.运行：cd vue3_test_vite；
3.运行：npm install；
4.运行：npm run dev；
5.访问服务器：http://localhost:3000



#### 139-分析Vue3的工程结构

Vue3-CLI创建工程的结构分析：
1.main.js中变化：引入的不再是Vue构造函数了，而是一个createApp工厂函数来创建vm对象，实现代码如下：
```
import { createApp } from 'vue'
import App from './App.vue'
createApp(App).mount('#app')


//上述代码中，使用createApp函数创建vue实例对象app，这里的app和vue2中的vm更轻（即：app中不像vm中包含很多的属性和方法）

```
2.Vue3中可以不在template中放入div等根标签；



#### 140-Vue3开发者工具的安装（略）



//截止到2022.9.3日晚20：28




