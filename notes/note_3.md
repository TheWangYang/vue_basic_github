## 尚硅谷Vue系列教程学习笔记（3）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。



#### 021-天气案例

下面使用计算属性实现了，点击按钮切换天气状态的功能。

```


  <div id="root">
        <h2>今天天气很好，是{{info}}！</h2>
        <!--下面也可以直接来在click方法后面写js表达式，但是只限于较为简单的方法，如果表达式较为复杂，那么就不推荐写到html标签中-->
        <button @click="isHot = !isHot">切换天气</button>
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false


        new Vue({
            el:'#root',
            data:{
                isHot:true,
            },
            computed:{
                info(){
                    return this.isHot ? '炎热' : '凉爽'
                }
            },
            methods: {
                change(){
                    //this.isHot = !this.isHot  //这里在methods中设置对vm中变量isHot的改变，之后在computed计算书型中也会重新计算然后返回结果
                }
            },
        })
    </script>


```


#### 022-监视属性
配置vm中的watch属性监视的两种方式：
```

const vm = new Vue({
            el:'#root',
            data:{
                isHot:true,
            },
            computed:{
                info(){
                    return this.isHot ? '炎热' : '凉爽'
                }
            },
            methods: {
                change(){
                    this.isHot = !this.isHot
                }
            },
            // watch:{
            //     isHot:{
            //         immediate:true,//初始化时让handler调用一下
            //         handler(newValue,oldValue){
            //             console.log("isHot被修改了！",newValue,oldValue)
            //         }
            //     }
            // }
        })

        //另外一种设置监视属性的方法
        vm.$watch('isHot',{
            immediate:true,
            handler(newValue, oldValue){
                console.log("isHot被修改了！",newValue,oldValue)
            }
        })


```


上述两种方式调用的条件：
1.创建vm时不知道需要监视谁的时候，选择第二种方法；
2.创建vm时就知道需要监视的属性时，选择第一种方法；


总结一下监视属性：
1.当监视的属性变化时，回调函数自动调用，进行相关操作；
2.监视的属性必须存在，才可以进行监视；
3.监视属性的两种写法：newValue传如watch配置，vm.$watch()设置；



需要注意的是：监视的属性不存在时，Vue不会报错，但是打印newValue和oldValue都是undifined；




#### 023-深度监视

Vue中实现对多级变量的监视代码：


```

watch:{
                isHot:{
                    // immediate:true,//初始化时让handler调用一下
                    handler(newValue,oldValue){
                        console.log("isHot被修改了！",newValue,oldValue)
                    }
                },
                //监视多级结构中某个属性的变化
                'numbers.a':{
                    handler(){
                        console.log("a的值改变了！")
                    }
                }
            }


```



下面为监视多级结构中所有属性的变化代码：
```
 watch:{
                isHot:{
                    // immediate:true,//初始化时让handler调用一下
                    handler(newValue,oldValue){
                        console.log("isHot被修改了！",newValue,oldValue)
                    }
                },
                //监视多级结构中所有属性的变化
                numbers:{
                    deep: true,
                    handler(){
                        console.log('numbers改变了！')
                    }
                }
            }


```

使用watch监视属性时，需要注意数据的结构来决定是否需要添加deep:true来监视深度属性。


#### 024-监视的简写形式

监视的简写条件：当watch属性的配置项中只有handler()方法时，可以简写。


下面总结的两种不同的watch方式的简写形式：

```
//在vm对象实例中使用watch函数
 watch:{
                //完整形式
                // isHot:{
                //     // immediate:true,//初始化时让handler调用一下
                //     handler(newValue,oldValue){
                //         console.log("isHot被修改了！",newValue,oldValue,this)  //这里的this是vm对象实例
                //     }
                // }


                //简写形式
                // isHot(newValue,oldValue){
                //     console.log('isHot被修改了！',newValue,oldValue,this)  //这里的this是vm对象实例
                // }

            }


//使用vm.$watch对象进行配置的两种方式

//使用$的正常形式
        vm.$watch('isHot',{
            immediate:true,//初始化时让handler调用一下
            handler(newValue,oldValue){
                console.log("isHot被修改了！",newValue,oldValue,this)  //这里的this是vm对象实例
            }
        })

        //使用$的简写形式
        vm.$watch('isHot',function(newValue,oldValue){
            console.log('isHot被修改了！', newValue, oldValue)  //这里的this是vm对象实例
        })


```



#### 025-监视属性与computed计算属性的比较

下面代码使用监视属性实现姓名案例：

```

<body>

    <!--准备好一个容器-->

    <div id="root">
        姓：<input type="text" placeholder="请输入姓" v-model="first_name" /> </br>
        名：<input type="text" placeholder="请输入名" v-model="second_name" /> </br>

        姓名：<span>{{fullName}}</span>
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false



        const vm = new Vue({
            el: '#root',
            data: {
                first_name: "张",
                second_name: "三",
                fullName: "张-三"
            },
            watch: {
                first_name(newValue, oldValue) {
                    this.fullName = newValue + "-" + this.second_name
                },
                second_name(newValue, oldValue) {
                    this.fullName = this.first_name + "-" + newValue
                }
            }
        })

    </script>

</body>


```



需求：用户输入之后1s后改变显示结果：
使用watch监视属性可以很方便地实现异步交互动作。
```
const vm = new Vue({
            el: '#root',
            data: {
                first_name: "张",
                second_name: "三",
                fullName: "张-三"
            },
            watch: {//不需要接受返回值，只需要写代码对需要更改的数据进行更改就行
                first_name(newValue) {
                    setTimeout(()=>{//直接设置定时器即可
                         this.fullName = newValue + "-" + this.second_name
                    },1000)
                    //this.fullName = newValue + "-" + this.second_name
                },
                second_name(newValue) {
                    this.fullName = this.first_name + "-" + newValue
                }
            }
        })


```


但是使用计算属性不可以实现异步交互，因为计算属性不能得到定时器中的return 返回值：
```
//下面的代码是不行的，因为computed函数得不到return的返回值，也就是说：计算属性是不能通过异步动作来得到返回值的。

computed: {
                fullName() {
                    setTimeou(() => {
                        return this.first_name + "-" + this.second_name
                    }, 1000)

                    // return this.first_name + "-" + this.second_name
                }
            }

```

总结：当面临一些异步的计算过程时，需要使用watch属性。


重点：定时器调用不是Vue对象实例进行的，而是浏览器调用的。


对于watch中调用setTimeout函数中this对象的理解：

```

watch: {
                first_name(newValue) {
                    setTimeout(() => {
                        console.log(this)  //此时的this为vm对象实例
                        this.fullName = newValue + "-" + this.second_name
                    }, 1000)
                    // this.fullName = newValue + "-" + this.second_name
                },
                second_name(newValue) {
                    this.fullName = this.first_name + "-" + newValue
                }
            },



//如果写成了如下代码：

watch: {
                first_name(newValue) {
                    setTimeout(function(){//不使用=>而是function()的写法，那么this为windows对象
                        console.log(this)  //此时的this为windows对象实例
                        this.fullName = newValue + "-" + this.second_name
                    }, 1000)
                    // this.fullName = newValue + "-" + this.second_name
                },
                second_name(newValue) {
                    this.fullName = this.first_name + "-" + newValue
                }
            },


```

重点：一定注意this对象指向的对象实例是哪一种。


总结-computed和watch之间的区别：
1.computed能完成的，watch都可以完成；
2.watch能完成的，computed不一定能完成；
3.所有被Vue管理的函数，最好写成普通函数，这样其中this对象才为vm对象或组件实例对象；
4.所有不被Vue管理的函数最好写成：=>函数，这样this的指向才是vm对象或组件实例对象；



#### 026-绑定class样式


下面的代码实现了绑定多个style样式的不同方法：
1.使用字符串方式绑定（适用于样式类名不确定的情况）；
2.使用数组方式绑定（适用于要绑定的样式个数、名字不确定的情况）；
3.使用对象方式绑定（适用于要绑定的样式个数确定、名字确定，但是使用不使用不确定）；




```

<body>

    <!--准备好一个容器-->

    <div id="root">
        <!--绑定class样式，字符串写法，适用于样式的类名不确定需要动态指定的情况-->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div>

        </br>
        </br>

        <!--实现class绑定任意数量的样式，数组样式，比较方便的删除和添加-->
        <div class="basic" :class="arr" @click="changeMood">{{name}}</div>

        </br>
        </br>

        <!--需求：test1和test2的两种样式的使用与不使用的四种组合方式-->
        <!--绑定方式：对象写法。适用于要绑定的样式个数确定，名字确定，但是使用不使用不确定的情况-->
        <div class="basic" :class="classObj" @click="changeMood">{{name}}</div>



    </div>


    <script type="text/javascript">
        Vue.config.productionTip = false


        new Vue({
            el:"#root",
            data:{
                name:"test",
                mood:"normal",
                arr:['test1','test2','test3'],
                classObj:{//对象方式绑定样式
                    test1:false,
                    test2:true
                }
            },

            methods:{
                changeMood(){
                    //设置随机切换心情mood
                    const moodArr = ['happy', 'normal','sad']
                    //使用随机数生成0，1,2三个整数
                    const index = Math.floor(Math.random() * 3)  //向下取整之后得到的index索引值
                    this.mood = moodArr[index]
                }
            }
        })


    </script>



</body>


```




#### 027-绑定style样式
使用vm中的数据对象进行绑定：


```

<div class="basic" :style="styleObj">{{name}}</div>


//js代码如下
 new Vue({
            el: "#root",
            data: {
                name: "test",
                mood: "normal",
                arr: ['test1', 'test2', 'test3'],
                classObj: {
                    test1: false,
                    test2: true
                },

                styleObj: {
                    fontSize: '40px'
                }

            }
        })

```


使用多个style样式的写法：

```

<div class="basic" :style="[styleObj1,styleObj2]">{{name}}</div>


//js代码如下

new Vue({
            el: "#root",
            data: {
                name: "test",
                mood: "normal",
                arr: ['test1', 'test2', 'test3'],
                classObj: {
                    test1: false,
                    test2: true
                },

                //运用多个style样式的代码段
                styleObj1: {
                    fontSize: '40px'
                },
                styleObj2: {
                    fontSize: '40px',
                    color:'blue'
                }

            },

            // methods: {
            //     changeMood() {
            //         //设置随机切换心情mood
            //         const moodArr = ['happy', 'normal', 'sad']
            //         //使用随机数生成0，1,2三个整数
            //         const index = Math.floor(Math.random() * 3)  //向下取整之后得到的index索引值
            //         this.mood = moodArr[index]
            //     }
            // }
        })


```




#### 028-条件渲染


使用Vue实现条件渲染的方法为：
```

    <h1>n的值为{{n}}</h1>
    <button @click="n++">点我n+1</button>

    <div v-if="n === 1">Angular</div>
    <div v-if="n === 2">React</div>
    <div v-if="n === 3">Vue</div>


//js代码为

 <script type="text/javascript">
        Vue.config.productionTip = false

        new Vue({
            el:"#root",
            data:{
                name:"test",
                n:0
            }
        })
    </script>
//即在js中的vm对象中指定条件n变量即可

```





重点：如果需要判断的值切换频率高使用v-show，低的使用v-if，因为v-show只是在dom节点中控制显示与否，if是不断地在dom节点中添一个结点或删一个结点。
上述只是推荐使用。


使用v-if时，如果在第一个v-if之后使用v-else-if，那么模板解析的效率会高一点：

```

 <div v-if="n === 1">Angular</div>
    <div v-else-if="n === 2">React</div>
    <div v-else-if="n === 3">Vue</div>

//解释原因：因为当使用了v-else-if之后，如果第一个v-if满足条件的话，及时后边的v-else-if也满足条件，但代码不会继续执行了。
```



使用template标签配合v-if实现条件渲染，需要注意的是，template只能配合v-if使用。
```

 <template v-if="n === 1">
        <h2>你好</h2>
        <h2>西安交大</h2>
        <h2>西安</h2>

    </template>



```



#### 029-列表渲染


下面为遍历列表代码：

```

<body>

    <!--准备好一个容器-->

    <div id="root">

        <ul>
            <!--让每一个li都有一个id标识-->
            <li v-for="p in persons" :key="p.id">
                {{p.name}}-{{p.id}}-{{p.age}}
            </li>
        </ul>

    </div>


    <script type="text/javascript">
        Vue.config.productionTip = false


        new Vue({
            el: "#root",
            data: {
                persons: [
                    { id: '001', name: '张三', age: 22 },
                    { id: '002', name: '李四', age: 24 },
                    { id: '003', name: '王五', age: 26 }
                ]
            }
        })

    </script>
</body>


```


下面为遍历对象的代码：


```
<!--遍历对象-->
        <h2>汽车配置属性值</h2>
        <ul>
            <li v-for="(value,k) of cars" :key="k">
                {{value}}-{{k}}
            </li>

        </ul>

//js代码
//vm的data属性中添加一个对象即可

cars:{
     car_name:"布加迪",
     car_price:"200w",
    car_id:"001"
}


```


下面为遍历字符串代码：

```


 <!--遍历字符串-->
        <h2>遍历字符串</h2>
        <ul>
            <li v-for="(char, index) of str" :key="index">
                {{char}}-{{index}}
            </li>
        
        </ul>

```

下面为遍历指定次数的代码：

```

<!--遍历指定次数-->
        <h2>遍历指定次数</h2>
        <ul>
            <li v-for="(number, index) of 10" :key="index">
                {{number}}-{{index}}
            </li>
        
        </ul>

```



#### 030-key的作用与原理


重点：Vue对于key属性的处理，在使用之后生成dom之后，就删掉了。


一些属性作为key时，对li中新增加的数据的显示可能会出现不同的问题：
1.使用index时会出现这种情况：在数组head处增加一个数据时，原来页面中的其他数据对应的内容不会随着移动，这就导致了错位现象的发生；也就是说，如果采用index作为页面dom的key属性，且插入的新数据破坏了原来数组数据中的顺序，那么就会导致错位现象的发生；
2.不使用key属性时，也会出现上述的情况；
3.使用数据本身的id（如：p.id）来作为key时，不会出现上述的情况；


解释上述情况出现的原因需要清楚以下两个方面：
1.虚拟dom的对比原理；
2.key的工作原理；


重点：如果没有特别指定key的值，那么Vue自动将index索引值自动作为key值。



总结一下key的原理和虚拟DOM的原理：
1.虚拟DOM中key的作用：
key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据[新数据]生成[新的虚拟DOM]
随后Vue进行[虚拟DOM]与[旧虚拟DOM的对比]的差异比较，比较规则如下：
    对比规则：
        （1）旧虚拟DOM中找到了与新虚拟DOM对象中一样的key的：
            如果虚拟DOM中key没有改变，那么直接使用之前的真实DOM；
            如果虚拟DOM中key发生改变，那么生成新的真实DOM，随后替换掉页面中之前的真实DOM
        （2）旧虚拟DOM中未找到新虚拟DOM中的相同的key：
            创建新的真实DOM，随后渲染到页面
    

    用index作为key可能会引发的问题：
        1.若对数据进行：逆序添加，逆序删除等破坏顺序操作：
            会产生没有必要的真实DOM更新=>界面效果没有问题，但效率低；
        2.如果结构中还包含了输入类DOM：
            会产生错误DOM更新=>界面有问题
    

    并发中如何选择key？
        1.最好使用每条数据的唯一标识id作为key，比如：id，手机号，身份证号，学号等；
        2.如果不存在对数据的逆序添加，逆序删除等破坏数据顺序的操作，仅用于渲染列表，那么使用index作为key也是可以的。


//截止到2022.8.15晚 22：39




