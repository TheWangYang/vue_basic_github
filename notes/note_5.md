## 尚硅谷Vue系列教程学习笔记（5）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。




#### 041-v-html指令


使用v-text指令无法解析data数据中存在的str:"h2 你好 /h2 "，其中h2表示h2标签，而使用v-html指令可以实现对上述中str的解析。

也就是说：v-text不支持结构的解析，v-html支持结构的解析。


服务器给的cookie不能跨浏览器使用的。


cookie很重要不能丢失。


如果服务器返回cookie时，没有添加HttpOnly属性，那么就有些cookie可以使用document.cookie获得，然后通过跳转的链接，传入到其他的网站。


一定要在可信的网站上使用v-html，永不要用在用户提交的内容上。

在网站上使用v-html动态渲染时，会导致XSS攻击。



#### 042-v-cloak指令

js阻塞：在页面开头的jS代码如果无法执行，那么后边的html标签和js片段也无法执行。

v-cloak属性会在Vue接管容器的一瞬间被删除掉。


v-cloak需要配合css使用，示例代码如下：

```

<html>

<head>
    <meta charset="UTF-8" />
    <title>v-cloak指令</title>

    <style>
        [v-cloak]{
            display: none;
        }

    </style>

    <script type="text/javascript" src="../js/vue.js"></script>


</head>

<body>

    <!--准备好一个容器-->

    <div id="root">
        <div v-cloak>你好：{{name}}</div>
    </div>




    <script type="text/javascript">
        Vue.config.productionTip = false

        new Vue({
            el:"#root",
            data:{
                name:"test"
            }
        })



    </script>
</body>


</html>

```
解析：
上述代码中，引入Vue对象的位置在html标签之后，如果不加v-cloak属性的话，html页面会首先渲染出来，但是出现的是：你好：{{name}}，因为此时Vue并没有被引入，但当Vue.js引入之后，会出现成功渲染的结果。因此，使用v-cloak属性配合css代码，指定在Vue.js引入之前，将{{name}}先设置为：display:none;这样就可以避免在未引入Vue.js时出现的页面结构呈现给用户的后果。


#### 043-v-once属性



v-once的使用案例：
```

 <h2 v-once>初始化的n值为：{{n}}</h2>
        <h2>当前的n值为：{{n}}</h2>
        <button @click="n++">点我n+1</button>


```

上述代码中，v-once属性设置表示：
1.在节点初次渲染后，就视为动态内容了；
2.以后数据的改变不会影响v-once中的数据改变，可以用于优化性能；




#### 044-v-pre指令


v-pre属性：
1.跳过其所在节点的编译过程；
2.可利用它跳过没有使用指令语法/插值语法等内容的编译过程，加快编译速度；

```


    <!--准备好一个容器-->
    <div id="root">
        <h2 v-pre>测试v-pre指令</h2>
        <h2>当前的n值为：{{n}}</h2>
        <button @click="n++">点我n+1</button>
    </div>


```



#### 045-自定义指令-函数式


自定义函数何时会被调用？
1.指令和数据成功绑定时会被调用；
2.指令所在的模板被重新解析时会被调用；


自定义指令就是操作DOM中的：属性/内容/事件等。


Vue中自定义指令-函数式案例代码：
```
//html代码
<div id="root">
        <h2>当前n的值为：<span v-text="n"></span></h2>
        <h2>放大10倍的n值为：<span v-big="n"></span></h2>
        <button @click="n++">点我n+1</button>
    </div>



//Vue对象实例中的js代码
new Vue({
            el: "#root",
            data: {
                n:1
            },
            directives:{
                big(element,binding){
                    element.innerText = binding.value * 10
                    // console.log(element, binding.value)  //这里的a表示真实DOM对象，这里的b表示
                }
            }
        })

```





#### 046-对象式，

对象式案例：实现在页面加载出input输入框时，input输入框中显示n的值，且input输入框获得输入焦点：

```

//html代码
<div id="root">
        <h2>当前n的值为：<span v-text="n"></span></h2>
        <h2>放大10倍的n值为：<span v-big="n"></span></h2>
        <button @click="n++">点我n+1</button>

        <!-- <input type="text" v-bind:value="n"> -->
        <input type="text" v-fbind:value="n">
    </div>


//Vue中js代码

new Vue({
            el: "#root",
            data: {
                n:1
            },
            directives:{
                big(element,binding){
                    element.innerText = binding.value * 10
                    // console.log(element, binding.value)  //这里的a表示真实DOM对象，这里的b表示
                },

                // fbind(element, binding){//fbind使用这种写法时，不会直接在初次加载页面时就获取焦点
                //     element.value = binding.value
                //     element.focus()
                // }
                fbind:{
                    //f指令与页面绑定之后调用
                    bind(element, binding){
                        element.value = binding.value
                    },
                    //指令所在元素被插入页面时调用
                    inserted(element, binding){
                        element.focus()
                    },
                    //指令所在模板被重新解析时调用
                    update(element, binding){
                        element.value = binding.value
                        element.focus()
                    }

                }
            }
        })

```


上述实现fbind对象式中，使用了三个函数：bind()/inserted()/update()，这三个函数有不同的调用时机（具体见上述代码）。





#### 047-自定义指令-总结



自定义指令的最完整写法：
```
//html代码
<input type="text" v-big-number:value="n">



//js代码

directives:{
                //最完整写法
                'big-number':function(element, binding){
                    
                }
            }

```

重点：对象里面的key出现了'-'，就不可以使用定义自定义指令函数的简写形式，需要写为完整的形式。


所有指令相关的函数中的this都是window对象。


在一个Vue对象中的所有指令只能在当前Vue对象中使用，而在其他的Vue对象中无法使用。


Vue中定义全局自定义指令的代码：
```
Vue.directive('fbind',{
            bind(){

            },
            
            inserted(){

            },

            update(){

            }
        })

```


重点：自定义指令命名时，不能使用驼峰命名法，而是需要使用'-'将多个单词隔开。





#### 048-引出生命周期

Vue中提供的mounted方法可以在Vue完成DOM解析并将初始的真实DOM放入页面之后调用。


使用案例：

```
//html代码
<div id="root">
        <h2 :style="{opacity}">欢迎学习Vue技术</h2>
    </div>


//js代码
const vm = new Vue({
            el:"#root",
            data:{
                opacity:1
            },
            // methods: {//methods无法实现渐变功能
            //     changeOpacity(){

            //         //设置内部定时器
            //         setInterval(() => {
            //             this.opacity -= 0.01
            //             if(this.opacity <= 0){
            //                 this.opacity = 1
            //             }
            //         },16)
            //     } 
            // },


            //挂载函数
            mounted() {
                //设置内部定时器
                    setInterval(() => {
                        this.opacity -= 0.01
                        if(this.opacity <= 0){
                            this.opacity = 1
                        }
                    },16)
                
            },
        })

```



Vue中提供的生命周期函数不可以更改名字，但是程序员可以在其中实现一些特定的逻辑。


生命周期函数中的this指定的是vm对象和组件实例对象。




#### 049-挂载过程

在Vue初始化期间，创建虚拟DOM过程中，如果没有指定el绑定的组件，那么在调用vm.$mount('#root')之后，Vue的生命周期才得以继续进行。


Vue中如果写了template属性，那么其中的html标签会完全替换掉el绑定的html组件中的所有标签元素。


总结一下Vue的生命周期，以及详细地分析：

```

 <script type="text/javascript">
        Vue.config.productionTip = false

        const vm = new Vue({
            el:"#root",
            data:{
                n:1
            },
            methods: {
                add(){
                    this.n ++
                }
            },
            beforeCreate() {
                console.log("beforeCreate")
                // console.log(this)
                // debugger;
            },

            created(){
                console.log('created')
                // console.log(this)
                // debugger;
            },
            beforeMount() {//此时页面模板为还没有被Vue编译的结构，其中的函数等动作均无效
                //此时对DOM组件的操作均为无效的
                console.log('beforeMount')
                console.log(this)
                // debugger;
            },
            mounted() {//到这里，Vue的初始化过程结束
                //此时对DOM的操作是可以实现的，但是尽可能避免操作DOM，在这里一般进行如下动作：
                //开启定时器，发送网络请求，订阅消息，绑定自定义事件等初始化操作
                console.log('mounted')
                // console.log(this)
                debugger;
            },
        })
    </script>

```

#### 050-更新流程


Vue中不能使用`<template>`标签作为template属性的根标签，因为使用`<template>`时，其中可能包含多个nodes节点组件。


Vue中的更新流程代码和详细解释：

```
beforeUpdate() {//数据是新的，但是页面中没有更改，数据和页面显示不同步
                console.log("beforeUpdate")
                console.log(this)
                // debugger;
            },

            updated() {//此时，数据是新的，页面也是新的
                console.log("updated")
                console.log(this.n)

            },
```

//截止到2022.8.17晚上21：42止




















