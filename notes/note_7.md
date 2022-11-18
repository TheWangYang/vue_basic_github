## 尚硅谷Vue系列教程学习笔记（7）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)



#### 061-创建Vue脚手架

Vue脚手架是一个官方提供的标准化的开发工具。


Vue CLI中的CLI表示：command/line/interface。


使用npm包管理和vue-cli脚手架创建项目的案例代码：

```

1.npm create vue_demo，一路回车，之后等待项目创建成功。
2.npm install（如果需要安装一些依赖包的话，使用该命令）
3.npm run serve（运行刚刚创建好的vue_demo项目）

```

#### 062-分析脚手架结构

Vue-CLI创建的项目结构：
1.gitgnore：git配置文件，包含：设置哪些文件push，哪些文件不允许push上传等；
2.babel.config.js：babel的控制文件；
3.package.json：只要项目是由npm创建的，都有这个文件，包含了安装包的说明和项目的相关配置属性等；
    其中包含的一些命令解释：
        vue-cli-service serve:表示将Vue项目配合自动生成的服务器在浏览器中使用；
        vue-cli-service build:表示最后在完成了Vue文件的编写之后，调用该命令完成最后的build构建，生成.html/.css/.js文件等；
        vue-cli-service lint:执行之后，Vue会对项目中的所有.Vue/.js等文件的语法进行一遍检查；
4.package-lock.json：是安装包版本控制文件；
5.README.md文件：对项目的说明；
6.src/main.js文件：
```
/**
 * 该文件是整个项目的入口文件
 * **/

//引入Vue
import { createApp } from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'

//创建Vue实例对象
createApp(App).mount('#app')

```


7.App.vue文件：
```
<template>
<div>
    <img src="./assets/logo.png" alt="logo">
    <!--组件的结构-->
    <School></School>
    <Student></Student>
</div>
    
</template>


<script>
//引入组件
import School from './components/School.vue'
import Student from './components/Student.vue'

    //另外一种不写Vue.extend()的方法
    export default {
        name:"App",//定义组件的名称
        components:{
            Student,
            School
        }
    }

</script>

```

8.assets目录：存放静态资源，便于项目合作开发者同时使用其中的资源文件。

9.components目录：所有新创建的组件放入到该目录下；

10."<%= BASE_URL %>favicon.ico"相当于"./favicon.ico"，这里的<%= BASE_URL %>就相当于./

11.Vue-CLI项目中的public目录下放入的index.html文件中的配置解析：


```

<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <!--使IE浏览器以最高级别渲染页面-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!--开启移动端的理想视口-->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!--设置浏览器中页面title中显示的图标-->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <!--配置网页标题-->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <!--如果浏览器不支持js，那么该提示就会出现在页面上-->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>

    <!--容器：用来接收放入的组件-->
    <div id="app"></div>

    <!-- built files will be auto injected -->
  </body>
</html>

```



#### 063-render函数


重点：在Vue-CLI脚手架项目中，App.vue文件中引入的vue文件为：

```
 "module": "dist/vue.runtime.esm-bundler.js",

```
上述的vue.runtime.esm-bundler.js文件是不完整的vue，对App.vue中使用template的地方无法解析，解决办法：
1.将放入到模板中的代码放入到render函数中；
2.使用完整版的带有模板解析功能的Vue.js文件；


重点：render函数详细解析

```
render(createElement){
    //参数的使用
    createElement('h1','你好啊！')
}

//另外一种写法
render:(createElement) => {
    //参数的使用
    createElement('h1','你好啊！')
}

//对上述另外一种写法的简写方法
render:createElement=> createElement('h1','你好啊！')

//继续简写
render:h => h('h1','你好啊！')


//render函数中传入的参数为：createElement

```


重点：Vue包含核心+模板解析器，且模板解析器占Vue文件的总体积为Vue文件总体积的将近三分之一。所以使用精简版的Vue文件的目的是：在Vue使用webpack打包完成之后，Vue文件中包含的模板解析器没有作用了，这就是使用精简版的不包含模板解析器的Vue的原因。


vue.runtime.js系列Vue文件是Vue运行时的js文件，体积均小于Vue.js文件。


注意 ：.Vue文件中的`<template>`标签的解析由package.json文件中引入的模块：vue-template-compiler进行实现。



#### 064-修改默认配置
使用如下命令得到Vue-CLI项目的所有配置：
```
>vue inspect > output.js

```

重点注意：output.js文件只是将当前项目中的所有配置修改一下，而不是项目的整体配置文件，修改该文件不会反映到项目中。


重点：采用Vue-CLI脚手架生成的项目中不可以修改的文件：
1.public目录不可修改，index.html/favicon.ico不能修改；
2.src不能改；
3.main.js不能改；


重点：Vue.config.js文件是可选的，需要对其修改时，要将该文件放入和package.json文件同级目录下。

程序员在对vue.config.js文件中进行修改之后，vue会将其中修改的部分放入到webpack中的vue.config配置文件中，进行自动合并。


如果vue.config.js文件中什么都不写的话，在运行npm run serve时会出现错误，因为Vue会将空的这个配置文件放入到webpack中。


Vue-CLI脚手架内置了语法检查功能，如果js代码中出现了定义了值但是没有使用的变量，那么该项目是无法启动的。

但是在开发的时候可以先将语法检查去掉，那么直接在vue.config.js文件中进行配置即可：
```

const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true
})

module.exports = {
  pages: {
    ...
  },
  lintOnSave:false  //关闭语法检查
}

```


#### 065-ref属性

重点：在App.vue文件的script标签中，使用this.$refs.xxx得到设置了ref属性的dom元素案例代码：

```
    <template>
    <div id="demo">
        <h1 v-text="msg" ref="title"></h1>
        <button ref="btn" @click="showDOM">点我显示DOM组件</button>

    <School/>
    <Student/>
    
    </div>
    </template>

    <script>

    import SCHOOL from "./components/School.vue"
    import STUDENT from "./components/Student.vue"

    export default {
        name:"App",
        components:{
            School:SCHOOL,
            Student:STUDENT
        },
        data(){
            return {
                msg:"测试消息"
            }
        },
        methods:{
            showDOM(){
                console.log("显示DOM",this.$refs.title)
                console.log("显示DOM",this.$refs.btn)

            }
        }
    }
    </script>

    <style>
    .demo{
        background-color: orange;
        font-size: 26px;
    }

    </style>

```

重点：你给哪个dom元素添加ref属性，那么vc对象就帮你收集那些dom元素。

注意：如果咋自定义单文件组件上添加ref，那么在js中输出的结果为：自定义单文件组件实例对象vc。

总结：
1.被用来给元素或子组件注册引用信息（id的替代者）；
2.应用在html标签上获取的是真实DOM元素，应用在组件标签上获得的是组件实例对象；
3.使用方式见上述代码块；




#### 066-props配置项
在Vue组件中使用props属性传参的案例代码：

1.简单接收的写法：

```
//Student.vue组件代码：

<template>
<div>
    <h2>学生姓名：{{studentName}}</h2>
    <h2>学生年龄：{{studentAge}}</h2>
</div>
</template>

<script>

export default {
    name:"STUDENT",
    data(){
        return {
            studentAge:33
        }
    },
    //告诉Vue组件我需要接收传来的那些属性
    props:['studentName']
}

</script>

<style>

</style>


//App.vue组件代码
    <template>
    <div id="demo">

    <h2 v-text="msg"></h2>

    <Student studentName="wyw" studentAge="22"/>
    
    </div>
    </template>

    <script>
    import STUDENT from "./components/Student.vue"
    export default {
        name:"App",
        components:{
            Student:STUDENT
        },
        data(){
            return {
                msg:"test测试消息"
            }
        }
    }
    </script>

    <style>
    .demo{
        background-color: orange;
        font-size: 26px;
    }

    </style>

```


上述代码中在Student.vue组件中设置了props属性，配置了其中需要接受的传参属性名，然后在App.vue组件里面使用如下代码进行传参和组件的调用：
```
//告诉Vue组件我需要接收传来的那些属性
    props:['studentName']

```



重点：对于number类型的属性，在传入参数时，使用组件的时候需要使用如下代码：
```

//使用学生年龄属性时，需要在前边添加一个:
<Student :StudentAge="18">

```


2.限制接收参数类型的写法：
```
//限制属性类别接收
    props:{
        studentName:String,
        studentAge:Number
    }
```


3.更加完整的接收参数方法
```

//更加完整配置的接收方法
    props:{
        studentName:{
            type:String,
            required:true
        },
         studentAge:{
            type:Number,
            required:true
         }
    }


```


重点：对于组件中采用props接受的属性值来说，是不可以修改的。


总结：props接受到的属性值比使用data() return{}更高。


细节问题：
Vue处理props属性值和data属性值的优先级对比为：前者先于后者的优先级。如果想要更改props属性值，需要在data属性中重新定义一个新的属性a，然后将该属性值设置为this.b，其中b为出现在props配置项中的某个属性值，然后在methods配置项中修改这个新定义的属性值a即可。




#### 067-mixin混入

重点：mixin：混入操作。


mixin的概念：混入的概念就是将两个组件中具有同样配置项的部分拿出来作为一个文件，然后两个组件中同时引用即可。



使用mixin案例代码：
```

//mixin.js文件代码
//将Student.vue和School.vue组件中配置项提取出来
export const hunhe = {
    methods: {
        showName(){
            console.log(this.name)
        }
    }
}



//Student.vue文件代码

<template>
<div>
    <h2 @click="showName">学生姓名：{{name}}</h2>
    <h2>学生年龄：{{age}}</h2>
</div>
</template>

<script>

import {hunhe} from "../mixin"

export default {

    name:"STUDENT",
    data(){
        return {
            name:"wyy",
            age:22
    }
    },
    // methods:{
    //     showName(){
    //         console.log(this.studentName)
    //     }
    // },
    mixins:[hunhe]
}

</script>

<style>

</style>



//School.vue文件代码
<template>
<div id="school">
    <h2 @click="showName">学校名称：{{name}}</h2>
    <h2>学校地址：{{address}}</h2>
</div>
</template>

<script>
import {hunhe} from "../mixin"
export default {
    name:"SCHOOL",
    data(){
        return {
            name:"xjtu",
            address:"西安"
        }
    },
    mixins:[hunhe]
}

</script>

<style>
.school{
    background-clip: blue;
}
</style>


```



全局混合配置代码：
```

//在main.js代码中进行配置
import Vue from "vue"
import { hunhe } from "./mixin"
Vue.mixin(hunhe)
```

重点：这样的话，在项目中所有的vc和vm对象实例中都会增加hunhe这一个混合代码。




#### 068-插件

插件的作用：对Vue项目进行增强。

重点：Vue中插件就是一个对象，但是这个对象中一定需要包含install方法。


使用插件的案例代码：
```
//plugins.js代码
export default {
    install() {
        console.log("测试插件！")
    }
}


//main.js文件中使用插件
//引入插件
import plugins from "./plugins"

//使用插件
Vue.use(plugins)


```


重点：在插件中可以定义全局过滤器/全局指令/全局混入/在Vue原型上添加一个方法等，能做的事情很多。



重点：install方法中：第一个参数为：Vue的构造者对象；第二个参数为：使用该插件使用者传入的数据；




#### 069-scoped样式


重点：当组件绑定的class冲突时，最后显示的样式取决于在App.vue中哪一个组件最后引入，那么样式就和最后一个引入的组件保持一致。


解决办法：为每个组件中的样式添加一个scoped标签即可。

```
//下面为Student.vue组件的样式设置
<template>
<div class="stuStyle">
    <h2 @click="showName">学生姓名：{{name}}</h2>
    <h2>学生年龄：{{age}}</h2>
</div>
</template>

<script>

export default {
    name:"STUDENT",
    data(){
        return {
            name:"wyy",
            age:22
        }
    }
}
</script>


<style scoped>
.stuStyle{
    background-color: antiquewhite;
}
</style>


```




#### 070-TodoList案例-静态

组件化编程流程：
1.实现静态组件，抽取组件，实现静态页面效果；
2.展示动态数据，（1）数据类型/名称；（2）数据保存在哪个组件；
3.交互-从绑定事件监听开始；



实现案例代码：
```
//MyHeader.vue组件
<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认">
    </div>
</template>

<script>
export default {
    name:"MyHeader",

}
</script>

<style scoped>

 /*header*/
        .todo-header input{
            width: 560px;
            height: 28px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 4px;
            padding: 4px 7px;

        }


        .todo-header input:focus{
            outline: none;
            border-color: rgba(82, 168, 236, 0.8);
            box-shadow: inset 1px 1px rgb(0, 0, 0, 0.75) 0 0 8px rgb(0, 0, 0, 0.5);
        }

</style>


//MyItem.vue组件
<template>
  <li>
    <label>
      <input type="checkbox"/>
      <span>xxxxx</span>
    </label>
    <button class="btn btn-danger" style="display: none">删除</button>
  </li>
</template>

<script>
export default {
  name: "MyItem",

}
</script>

<style scoped>
  /*item*/

        li{
            list-style: none;
            height: 36px;
            line-height: 36px;
            padding: 0 5px;
            border-bottom: 1px solid #ddd;
        }

        li label{
            float: left;
            cursor: pointer;
        }


        li label li input{
            vertical-align: middle;
            margin-right: 6px;
            position: relative;
            top: -1px;
        }


        li button{
            float: right;
            display: none;
            margin-top: 3px;
        }


        li:before{
            content: initial;
        }


        li:last-child{
            border-bottom: none;
        }

</style>


//MyList.vue组件
<template>
  <ul class="todo-main">
    <MyItem />
    <MyItem />
    <MyItem />
    <MyItem />
  </ul>
</template>

<script>
//引入Item组件
import MyItem from "../components/MyItem.vue";

export default {
  name: "MyList",
  components: {
    MyItem,
  },
};
</script>

<style>

/*list样式*/
.todo-main{
            margin-left: 0px;
            border: 1px solid #ddd;
            border-radius: 2px;
            padding: 0px;
        }

        .todo-empty{
            height: 40px;
            line-height: 40px;
            border: 1px solid #ddd;
            border-radius: 2px;
            border-left: 5px;
            padding-left: 5px;
            margin-top: 10px;
        }
        

</style>


//MyFooter.vue组件
<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" />
    </label>

    <span> <span>已完成0</span> / 全部2 </span>
    <button class="btn btn-danger">清楚已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
};
</script>

<style scoped>


        /*footer*/
        .todo-footer{
            height: 40px;
            line-height: 40px;
            padding-left: 6px;
            margin-top: 5px;
        }


        .todo-footer label {
            display: inline-block;
            margin-right: 20px;
            cursor: pointer;
        }


        .todo-footer label input{
            position: relative;
            top: -1px;
            vertical-align: middle;
            margin-right: 5px;
        }


        .todo-footer button{
            float: right;
            margin-top: 5px;
        }

</style>


//App.vue组件
    <template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
                <MyHeader/>
                <MyList/>
                <MyFooter/>

            </div>
        </div>
    </div>
    </template>

    <script>
    //引入组件
    import MyHeader from "./components/MyHeader.vue"
    import MyFooter from "./components/MyFooter.vue"
    import MyList from "./components/MyList.vue"

    export default {
        name:"App",
        components:{
            MyHeader,
            MyFooter,
            MyList
        }
        
    }
    </script>

    <style>
        body{
            background:#fff;
        }

        .btn{
            display:inline-block;
            padding: 4px 12px;
            margin-bottom: 0;
            font-size: 14px;
            line-height: 20px;
            text-align: center;
            vertical-align: middle;
            cursor: pointer;
            box-shadow: inser 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.2);
            border-radius: 4px;
        }

        .btn-danger{
            color: #fff;
            background-color: #da4f49;
            border: 1px solid #bd362f;
        }

        .btn-danger:hover{
            color: #fff;
            background-color: #bd362f;
        }

        .btn:focus{
            outline: none;
        }


        .todo-container{
            width: 600px;
            margin: 0 auto;
        }

        .todo-container .todo-wrap{
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }


    </style>


```


上述展示了使用组件来设置页面的代码。


//截止到2022年8月21日下午17:05止






