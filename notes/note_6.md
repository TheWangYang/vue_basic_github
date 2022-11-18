## 尚硅谷Vue系列教程学习笔记（6）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。


#### 051-销毁流程


调用vm.$destroy()之后，尽管vm对象已经消失，但是vm渲染的效果依然还在。

调用vm.$destroy()函数之后，Vue会清除它与其他实例相关的连接，解绑它的全部指令和事件的监听器。
这里的'事件监听器'指的是自定义事件而不是DOM事件，对于原始的DOM事件，在vm销毁之后还是可以点击，且还可以进入函数体内部的。


beforeDistroy函数中，vm的所有动作和data和methods等都可以使用。，一般在此阶段会发生如下行为：
1.关闭定时器；
2.取消订阅消息；
3.解绑自定义事件等收尾操作；

在beforeDistroy函数中设置的所有动作都无法实现作用，页面数据也不会再更新。



#### 052-生命周期总结
对vm实例对象的生命周期来说，mounted和beforeDistroy是最重要的步骤。

也就是说，初始化的事情在mounted函数中完成，收尾的操作在beforeDistroy函数中完成。


重点提示：vm.$distroy()函数尽量不要调用，可以使用v-if或v-elif指令来驱动控制组件。

注意：在给vm定义属性时，需要注意避开属性名为敏感属性名，例如：id/key等。



重点：点击按钮停止定时器控制组件透明度变换的实现代码：
```

//html代码
<div id="root">
        <h2 :style="{opacity}">欢迎学习Vue技术</h2>

        <button @click="stopChange">点我停止变换</button>
    </div>



//js代码

<script type="text/javascript">
        Vue.config.productionTip = false

        const vm = new Vue({
            el:"#root",
            data:{
                opacity:1
            },

            methods: {
                stopChange(){
                    //清楚定时器对象
                    //比较温柔的停止
                    // clearInterval(this.timer)

                    //直接销毁vm对象实例
                    this.$destroy()
                }
            },

            //挂载函数
            mounted() {
                //设置内部定时器
                //向this对象（vm）中存了一个id属性
                    this.timer = setInterval(() => {
                        this.opacity -= 0.01
                        if(this.opacity <= 0){
                            this.opacity = 1
                        }
                    },16)
                
            },
            //对定时器的销毁工作一般放到beforeDestroy()函数中进行
            beforeDestroy() {
                console.log('销毁定时器')
                //在销毁之前停止定时器
                clearInterval(this.timer)
            },
        })

    </script>

```

总结：
1.mounted中可以实现的动作：发送ajax请求/启动定时器/绑定自定义事件/订阅消息等；
2.beforeDestroy()中可以实现的动作：清除定时器/解绑自定义事件/取消订阅消息等；


关于销毁Vue实例：
1.销毁后，借助开发者工具，看不到任何消息；
2.清除后自定义事件会失效，淡水原生DOM事件依然有效；
3.一般不会在beforeDestroy中操作数据，因为即便操作数据，再不会触发更新流程了；


#### 053-对组件的理解

原始的HMTL编程中存在的问题：
1.依赖关系混乱，不好维护；
2.代码复用率不高；

组件里面包含：html/js/css。

组件体现的思想是：封装思想。


所有的组件都受到vm对象的指挥。

组件可以实现嵌套。

组件的定义：实现应用中局部功能代码和资源的集合。

模块：向外提供特定功能的js程序，一个模块就是一个js文件。作用：能够复用js代码，简化代码编写。


应用中的功能都是通过组件化的方式来写的时候，这个应用就是组件化应用。




#### 054-非单文件组件

注意：掌握了非单文件组件之后就会很自然地过渡到单文件组件。


非单文件组件：一个组件中包含多个组件。

单文件组件：一个文件中只包含有一个组件。


真正做开发时，用的都是单文件组件（条理清晰，代码好用）。

注意：一个.vue文件就是一个组件。



重点：组件在创建时，从来不和页面标签进行绑定，为了方便使用到其他地方。


组件在写data时一定要使用函数式，因为如果使用对象式，那么在他人修改data对象中的数据时，其他使用data数据的地方也会发生改变，而使用data()函数式调用时，每个不同的人使用data()得到函数中的返回数据时，都会创建一个新的对象，不同人创建的对象是不同的，因此不会相互影响。


使用组件的三步：
1.创建组件；
2.注册组件；
3.引入组件；


重点：一个组件在页面中多次使用是不产生干扰的。



重点：局部组件的使用方法：

```
//html代码
<div id="root">
        <!--使用局部组件-->
        <school></school>
        <student></student>
    </div>




//js代码
    <script type="text/javascript">
        Vue.config.productionTip = false


        //创建school组件
        const sch = Vue.extend({
            template:`
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{schoolAddress}}</h2>
                <button @click="alertSchoolName">点我提示学校名</button>
            </div>
            `,

            data(){
                return {
                    schoolName: "test",
                    schoolAddress: "test_address"
                }
            },
            methods:{
                alertSchoolName(){
                    alert(this.schoolName)
                }
            }

        })


        //创建student组件
        const stu = Vue.extend({
            template:`
            <div>
                <h2>学生姓名：{{studentName}}</h2>
                <h2>学生年龄：{{studentAge}}</h2>
            </div>
            `,
            data(){
                return {
                    studentName: "wyy",
                    studentAge: 22
                }
            }
        })

        //注册组件
        new Vue({
            el:"#root",
            //局部注册组件
            components:{
                school:sch,
                student:stu
            }
        })

    </script>
```



使用Vue.component全局注册组件代码：
```

//html代码
<div id="root">
        <!--使用全局注册的组件-->
        <hello></hello>
    </div>


//js代码
 <script type="text/javascript">
        Vue.config.productionTip = false

        //测试全局注册组件
        const hello = Vue.extend({
            template:`
            <div>
                <h2>你好！{{name}}</h2>    
            </div>
            `,
            data(){
                return {
                    name:"wyy"
                }
            }
            
        })

        //使用hello实现全局注册组件
        Vue.component('hello',hello)

    </script>

```


注意：使用全局注册组件实现了不同vm对象可以使用同一个组件的功能。



#### 056-组件的几个注意点


总结：在vm对象中定义组件名时，如果是两个字母的话中间使用'-'隔开，即为：'my-school':{}；同时，如果不使用-来区分开两个单词，可以使用如下形式：MySchool，但需要注意的是需要在vue-cli脚手架环境里使用这种写法。


注意：不使用vue-cli脚手架时，如果在页面标签中引入Vue组件时使用如下写法：`<school/>`，那么将会导致后续的页面不再进行渲染。

重点：
1.组件名尽可能回避和DOM中的标签重名；
2.使用组件中的name配置项更改组件在页面中使用的名字（即：不管组件在其他vm对象使用何种名字，但是最终在页面渲染出来的效果中现实的仍然是name配置项中的定义的名字）；


组件的简写形式：
```

const school = {
                template: `
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{schoolAddress}}</h2>

            </div>
            `,
                data() {
                    return {
                        schoolName: "xjtu",
                        schoolAddress: "西安"
                    }
                }
            }


```
上述代码中展示的组件简写形式同样可以实现的原因是：
在vm对象中的components配置项中有如下代码：
school:school
然后，Vue对传入的组件school进行判断，如果是一个对象，那么Vue可以自动帮助编写Vue.extend()，相反地，如果直接在定义组件的时候就写了Vue.edtend()，那么在components配置项中直接作为组件进行解析。




#### 056-组件的嵌套

注意：组件在哪里注册就在哪里使用即可。


组件嵌套使用的案例代码如下：
```
//html代码
 <div id="root">
        <school></school>

        <!-- <student></student> -->
    </div>


//js代码

<script type="text/javascript">
        Vue.config.productionTip = false



        //创建student组件
        const stu = Vue.extend({
            template:`
            <div>
                <h2>学生姓名：{{studentName}}</h2>
                <h2>学生年龄：{{studentAge}}</h2>
            </div>
            `,
            data(){
                return {
                    studentName: "wyy",
                    studentAge: 22
                }
            }
        })


        //定义school组件
        const sch = {
                template: `
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{schoolAddress}}</h2>
                <student></student>
            </div>
            `,
                data() {
                    return {
                        schoolName: "xjtu",
                        schoolAddress: "西安"
                    }
                },
                //在学校里面嵌入student组件
                components:{
                    student:stu
                }
            }
        

        //局部注册
        new Vue({
            el:"#root",
            components:{
                school:sch
            }
        })


    </script>



```


组件的嵌套综合案例代码：
```


<body>

    <!--准备好一个容器-->

    <div id="root">
        <!-- <school></school> -->
        <!-- <student></student> -->

        <app></app>

        <hr>

        <app></app>
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false


        const hello = Vue.extend({
            template:`
            <div>
                <h2>你好，{{helloName}}！</h2>    
            </div>
            `,
            data(){
                return {
                    helloName:"wyy"
                }
            }
        })


    
        //创建student组件
        const stu = Vue.extend({
            template:`
            <div>
                <h2>学生姓名：{{studentName}}</h2>
                <h2>学生年龄：{{studentAge}}</h2>
            </div>
            `,
            data(){
                return {
                    studentName: "wyy",
                    studentAge: 22
                }
            }
        })


        //定义school组件
        const sch = {
                template: `
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{schoolAddress}}</h2>
                <student></student>
            </div>
            `,
                data() {
                    return {
                        schoolName: "xjtu",
                        schoolAddress: "西安"
                    }
                },
                //在学校里面嵌入student组件
                components:{
                    student:stu
                }
            }


            //定义一个组合schoole和hello组件的app组件
            const app = Vue.extend({
                    template: `
            <div>
                <school></school>
                <hello></hello>    
            </div>
            `,
                    components: {
                        school: sch,
                        hello: hello
                    }
                })

        

        //局部注册
        new Vue({
            el:"#root",
            components:{
                // school:sch,
                // student:stu
                app:app
            }
        })


    </script>
</body>


```




#### 057-VueComponent

首先总结VueComponent：
1.school组件本身是一个名为VueComponent的构造函数，且不是程序员定义的，是由Vue.extend生成的；
2.我们只需要写`<school/>`或`<school><school>`，Vue解析时会自动帮我们创建school组件的实例对象，即Vue帮我们执行的：new VuewComponent(options)；
3.特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent对象，源码中也可以证明这一点：
```

var Sub = function VueComponent(options) {
              this._init(options);
          };


```


4.关于this指向：
    （1）组件配置中：
        data函数/methods中的函数/watch中的函数/computed中的函数，他们的this为VueComponent；
    （2）new Vue()配置中：
        data函数/methods中的函数/watch中的函数/computed中的函数，他们的this都是vue实例对象和组件实例对象；
5.VueComponent的实例对象，以后简称vc（也可称为：组件实例对象）。
Vue的实例对象，以后简称为vm。





#### 058-Vue实例与组件实例
vm可以使用el指定对哪个容器服务，但是vc不可以。





#### 059-一个重要的内置关系
原型对象解释：
```

<script type="text/javascript">
        Vue.config.productionTip = false


        //定义原型构造函数
        function Demo(){
            this.a = 1,
            this.b = 1
        }

        const demo = new Demo()

        console.log(Demo.prototype)  //显式原型属性
        console.log(demo.__proto__)  //隐式原型属性


    </script>

```

上述代码中，Demo.prototype和demo.__proto__都指向了一个原型对象属性。




重点：Vue中一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
为什么要有这个内置关系：让组件vc可以访问到vm对象中的属性、方法。



重点：实例对象的隐式属性永远指向缔造者的原型对象。


Vue让VueComponent原型对象的隐式属性指向了Vue的原型对象属性。





#### 060-单文件组件

解析xxx.vue文件的方法：
1.webpack方法；
2.借助Vue提供的脚手架；


定义单文件组件的命名规则：
如果是单文件组件，且只有一个单词命名，那么为：1.schoole.vue；2.School.vue；
多个单词作为名字时，为：1.my-school.vue；2.MySchool.vue；

重点：一般在开发当中喜欢选择上边两种方法中的各自第二种方式。


重点：
单组件文件的使用案例代码：

```

//Student.vue文件

<template>
    <!--组件的结构-->
    <div>
        <h2>学生姓名：{{studentName}}</h2>
        <h2>学生年龄：{{ studentAge }}</h2>
        <button @click="showName">点我提示学生姓名</button>
    </div>
</template>



<script>
    
    //另外一种不写Vue.extend()的方法
    export default {
        name:"Student",//定义组件的名称
        data(){
            return {
                studentName:"wyy",
                studentAge:18
            }
        },
        
        methods:{
            //点击显示学生姓名
            showName(){
                alert(this.studentName)
            }
        }
    }

</script>


//School.vue文件
<template>
    <!--组件的结构-->
    <div id="demo">
        
        <h2>学校名称：{{schoolName}}</h2>
        <h2>学校地址：{{ schoolAddress }}</h2>
        <button @click="showName">点我提示学校名称</button>
    </div>
</template>



<script>


    //另外一种不写Vue.extend()的方法
    export default {
        name:"School",//定义组件的名称
        data(){
            return {
                schoolName:"xjtu",
                schooleAddress:"西安"
            }
        },
        
        methods:{
            showName(){
                alert(this.schoolName)
            }
        }
    }

</script>



<style>
    /* 组件的样式 */

    .demo{
        background-color: orange;
    }



</style>




//App.vue文件
<template>
<div>
    <!--组件的结构-->
    <School></School>
    <Student></Student>
</div>
    
</template>



<script>
//引入组件
import School from './School.vue'
import Student from './Student.vue'

    //另外一种不写Vue.extend()的方法
    export default {
        name:"App",//定义组件的名称
        components:{
            Student,
            School
        }
    }

</script>


//main.js文件

//引入组件
import App from './App.vue'
new Vue({
    el: "#root",
    template: `
    <div>
        <App></App>
    </div>
    `,
    //添加组件
    components: {
        App
    }
})


//index.html文件
<!DOCTYPE html>
<html>
    <head>
        <title>练习单文件组件</title>
        <meta charset="UTF-8"/>
    </head>

    <body>
        <div id="root">
            
        </div>
        <script type="text/javascript" src="./main.js"></script>
        <script type="text/javascript" src="../js/vue.js"></script>
    </body>

</html>

```

上述代码无法在浏览器运行是因为没有放入到脚手架里面。


//截止到2022.8.19日中午12：16止

