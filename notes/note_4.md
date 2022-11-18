## 尚硅谷Vue系列教程学习笔记（4）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。




#### 031-列表过滤

实现对聊表的搜索的方法步骤：
1.收集用户的输入内容；
2.将收集到的内容应用到列表中；


案例—名字过滤：
```
//使用监视属性实现
<div id="root">

        <!--遍历数组-->
        <ul>
            <!--让每一个li都有一个id标识-->
            <h2>遍历列表</h2>
            <input type="text" placeholder="请输入想要搜索的名字：" v-model="keyWords">
            <li v-for="p in filPersons" :key="p.id">
                {{p.name}}-{{p.id}}-{{p.age}}
            </li>
        </ul>

    </div>

//js代码如下：

new Vue({
            el: "#root",
            data: {
                keyWords:"",
                persons: [
                    { id: '001', name: '张三', age: 22 },
                    { id: '002', name: '李四', age: 24 },
                    { id: '003', name: '王五', age: 26 },
                    { id: '004', name: '一哥', age: 26 }
            ],
            filPersons:[]
            },

            //用watch计算属性实现
            watch:{
                keyWords:{
                    immediate:true,
                    handler(val){
                         // console.log(val)
                        this.filPersons = this.persons.filter((p) => {
                            return p.name.indexOf(val) !== -1
                        })
                    }
                }
            }


        })




```



2.使用计算属性实现
```

<div id="root">

        <!--遍历数组-->
        <ul>
            <!--让每一个li都有一个id标识-->
            <h2>遍历列表</h2>
            <input type="text" placeholder="请输入想要搜索的名字：" v-model="keyWords">
            <li v-for="p in filPersons" :key="p.id">
                {{p.name}}-{{p.id}}-{{p.age}}
            </li>
        </ul>

    </div>


//js代码：

new Vue({
            el: "#root",
            data: {
                keyWords:"",
                persons: [
                    { id: '001', name: '张三', age: 22 },
                    { id: '002', name: '李四', age: 24 },
                    { id: '003', name: '王五', age: 26 },
                    { id: '004', name: '一哥', age: 26 }
            ]
            },

            //使用computed计算属性
            computed:{
                filPersons(val){
                    return this.persons.filter((p)=>{
                        return p.name.indexOf(this.keyWords) !== -1
                    })
                }
            }
        })


```


#### 032-列表排序


使用计算属性实现列表排序的案例：
```

<div id="root">

        <!--遍历数组-->
        <ul>
            <!--让每一个li都有一个id标识-->
            <h2>遍历列表</h2>

            <input type="text" placeholder="请输入想要搜索的名字：" v-model="keyWords">
            <button @click="sortType = 1">年龄升序</button>
            <button @click="sortType = 2">年龄降序</button>
            <button @click="sortType = 0">原顺序</button>

            <li v-for="p in filPersons" :key="p.id">
                {{p.name}}-{{p.id}}-{{p.age}}
            </li>
        </ul>

    </div>


//js代码
new Vue({
            el: "#root",
            data: {
                keyWords:"",
                sortType:0,  //0代表原顺序，1代表升序，2代表降序
                persons: [
                    { id: '001', name: '张三', age: 22 },
                    { id: '002', name: '李四', age: 24 },
                    { id: '003', name: '王五', age: 26 },
                    { id: '004', name: '一哥', age: 26 }
            ]
            },

            //使用computed计算属性
            computed:{
                filPersons(val){
                    //保留filPersons过滤后的数组
                    const arr = this.persons.filter((p)=>{
                        return p.name.indexOf(this.keyWords) !== -1
                    })

                    //然后进行排序
                    if(this.sortType !== 0){//表示需要排序
                        arr.sort((a,b)=>{
                            return this.sortType === 1 ? b.age- a.age : a.age - b.age
                        })
                    }

                    return arr
                }
            }
        })


```



#### 033-更新时的一个问题



更新时面对的问题：
```

 <div id="root">

        <!--遍历数组-->
        <ul>
            <!--让每一个li都有一个id标识-->
            <h2>遍历列表</h2>
            <button @click="updateSan">点击更新张三的信息</button>
            <li v-for="p in persons" :key="p.id">
                {{p.name}}-{{p.id}}-{{p.age}}
            </li>
        </ul>

    </div>

//js代码如下：
 <script type="text/javascript">
        Vue.config.productionTip = false
        //得到vm对象实例
        const vm = new Vue({
            el: "#root",
            data: {
                persons: [
                    { id: '001', name: '张三', age: 22 },
                    { id: '002', name: '李四', age: 24 },
                    { id: '003', name: '王五', age: 26 },
                    { id: '004', name: '一哥', age: 26 }
            ]
            },
            methods:{
                updateSan(){
                    this.persons[0] = {id:"000",name:"马老师",age:"54"}
                    console.log("已经更新完毕！")
                }
            }
        })

    </script>


```


上述代码中虽然点击按钮之后，在浏览器的控制台中调用vm.persons[0].name属性可以发现persons数组中的第一个项已经更改，但是Vue模板并没有监测到，这就是以这种方式进行更新时的一个问题。


具体原因解释详见034-Vue更新数据原理一节笔记。


#### 034-Vue监测数据的原理_对象



Vue实现监测数据原理，下面为较简单的Vue监测数据代码：


```
<body>

    <!--准备好一个容器-->

    <div id="root">

    </div>


    <script type="text/javascript">
        let data = {
            name:"test",
            address:"测试地址"
        }
        //创建一个监视的实例对象，用于监视数据的变化
        const obs = new Observer(data)

        //准备一个vm实例对象
        let vm = {}
        vm._data = data = obs

        function Observer(obj){
            //汇总对象中所有的属性形成一个数组
            const keys = Object.keys(obj)
            //遍历
            keys.forEach((k)=>{
                Object.defineProperty(this, k,{
                    get(){
                        return obj[k]
                    },

                    set(val){
                        console.log(`数据被修改了，vm需要重新生成新的虚拟DOM对象...`)
                        obj[k] = val
                    }
                })
            })
        }

    </script>
</body>


```
上述代码中，只要是存在data中的数据，Vue都会解析到最后不是对象为止，然后分别为每个对象生成一个get和set方法，然后在用户修改vm中数据对象时，set方法会被调用，然后set方法中还有更新真实DOM的逻辑，进而Vue实现更改data中数据对象之后，前端真实DOM中也会发生改变的功能。




#### 035-Vue.set()方法


重点：Vue中对undefined的属性值不显示。


对于在创建vm实例对象时没有设置的属性，在控制台中使用vm._data.sex = xxx之后，尽管在vm中存在新设置的sex属性，但是没有给sex属性创建get和set方法，因此Vue模板无法解析新创建的sex属性。


Vue不允许添加一个属性在vm实例身上，即下面的代码是不被Vue允许的：
```
Vue.set(vm._data,"sex","男")

```


使用Vue.set()方法实现点击之后给数据对象添加一个性别代码示例：


```

<div id="root">
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <h2>校长是：{{leader}}</h2>

        </br>

        <button @click="addAttribute">点我增加性别属性</button>
        <h2>学生姓名：{{student.name}}</h2>
        <h2>学生年龄：真实{{student.age.rAge}}, 对外：{{student.age.sAge}}</h2>
        <h2>学生性别：{{student.sex}}</h2>

        <ul v-for="f in student.friends" :key="f.id">
            <li>{{f.name}}-{{f.age}}</li>
        </ul>
    </div>



//js代码：
<script type="text/javascript">
        Vue.config.productionTip = false

        const vm = new Vue({
            el: "#root",
            data: {
                name: "姓名测试",
                address: "地址测试",
                student: {
                    id: "001",
                    name: "wyy",
                    age: {
                        rAge: 18,
                        sAge: 22
                    },
                    friends: [
                        { id: "002", name: "test1", age: 22 },
                        { id: "003", name: "test2", age: 32 },
                        { id: "004", name: "test3", age: 25 },
                    ]
                }
            },
            methods:{
                addAttribute(){
                    vm.$set(this.student,'sex','男')
                }
            }
        })


    </script>

```



#### 036-Vue监测数据的原理_数组


重点：Vue中不会对数组中的某个元素匹配get和set元素

Vue中更新数组数据的方法：

```


<div id="root">

        <!--遍历数组-->
        <ul>
            <!--让每一个li都有一个id标识-->
            <h2>遍历列表</h2>
            <button @click="updateSan">点击更新张三的信息</button>
            <li v-for="p in persons" :key="p.id">
                {{p.name}}-{{p.id}}-{{p.age}}
            </li>
        </ul>

    </div>


//js代码

<script type="text/javascript">
        Vue.config.productionTip = false
        //得到vm对象实例
        const vm = new Vue({
            el: "#root",
            data: {
                persons: [
                    { id: '001', name: '张三', age: 22 },
                    { id: '002', name: '李四', age: 24 },
                    { id: '003', name: '王五', age: 26 },
                    { id: '004', name: '一哥', age: 26 }
                ]
            },
            methods: {
                updateSan() {
                    //this.persons[0] = {id:"000",name:"马老师",age:"54"}
                    // console.log("已经更新完毕！")
                    this.persons.splice(0, 1, { id: "000", name: "马老师", age: "54" })
                }
            }
        })

    </script>


//即使用vm对象为数组数据提供的splice()方法即可
```



重点：vm._data.arr.push === Array.prototype.push，结果为false，因为此时vm对象中调用的push方法是Vue提供的一个新的push方法。

总结：Vue对数组的监测实际上是对数组身上常用的数组方法进行包装之后，进行监视的。





#### 037-总结Vue监视数据方法



总结上述Vue监视数据的方法案例代码：
```
<body>

    <!--准备好一个容器-->

    <div id="root">
        <h1>学生信息</h1>

        <h2>操作</h2>
        <button @click="student.age++">年龄增加+1岁</button>
        <button @click="addSex">添加性别属性，默认值：男</button>
        <button @click="changeSex">修改性别属性到与当前相反的值</button>
        <button @click="addFriend">在列表首位添加一个朋友</button>
        <button @click="changeFirstFriendName">修改第一个朋友的名字为：张三</button>
        <button @click="addHobby">添加一个爱好</button>
        <button @click="changeFirstHobby">修改第一个爱好为：开车</button>



        <h2>姓名：{{student.name}}</h2>
        <h2>年龄：{{student.age}}</h2>
        <h2>性别：{{student.sex}}</h2>

        <h2>该学生的爱好（多个）</h2>
        <ul v-for="h in student.hobby" :key="index">
            <li>{{h}}</li>
        </ul>


        <h2>该学生的朋友（多个）</h2>
        <ul v-for="f in student.friends" :key="f.id">
            <li>{{f.name}}-{{f.age}}</li>
        </ul>
    </div>


    <script type="text/javascript">
        Vue.config.productionTip = false

        const vm = new Vue({
            el: "#root",
            data: {
                sexType:false,//false表示女，true表示男
                student: {
                    name: "wyy",
                    age: 18,
                    hobby:["抽烟","喝酒","烫头"],
                    friends: [
                        { name: "test1", age: 22 },
                        { name: "test2", age: 33 },
                    ]
                }
            },
            methods: {
                addSex(){
                    Vue.set(this.student, "sex", "男")
                },
                changeSex(){
                    if(this.sexType === false){
                        this.sexType = true
                        this.student.sex = "男"
                    }else if(this.sexType === true){
                        this.sexType = false
                        this.student.sex = "女"
                    }
                },
                addFriend(){
                    this.student.friends.unshift({name:"test3",age:26})
                },
                changeFirstFriendName(){
                    this.student.friends[0].name = "张三"
                },
                addHobby(){
                    this.student.hobby.push("学习")
                },
                changeFirstHobby(){
                    // this.student.hobby[0] = "开车"
                    // this.student.hobby.splice(0, 1, "开车")
                    // this.$set(this.student.hobby,0,"开车")
                    Vue.set(this.student.hobby, 0, "开车")
                }

            }
        })


    </script>
</body>

```


文字总结：
1.Vue会监视data中所有层次的数据；
2.如何监视对象中的数据：
    通过setter实现监视，且要在new Vue时就传入要监测的数据；
    （1）对象中后追加的属性，Vue默认不做响应式处理；
    （2）如需给后添加的属性做响应式处理，使用如下代码：
        Vue.set(target,propertyName/index,value)
        vm.$set(target,propertyName/index,value)

3.如何监测数组中的数据：
    通过包裹数组更新元素的方法实现，本质就是做了两件事：
    （1）调用原生对应的方法对数组进行更新；
    （2）重新解析模板，进而更新页面；


4.在Vue中修改数组中间的某个元素一定要用如下方法：
    （1）使用API：push()/pop()/shift()/unshift()/splice()/sort()/reverse()
    （2）Vue.set()/vm.$set()

重点注意：Vue.set()/vm.$set() 不能给vm或vm的根数据对象 添加属性！！！





#### 038-收集表单数据

重点：Vue中对于checkbox类型的勾选框，在勾选时，需要给每个checkbox框设置value值，然后设置v-model绑定的属性为[]数组类型即可，代码如下所示。

```
爱好：
    学习<input type="checkbox" v-model="hobby" value="study">
    看电影<input type="checkbox" v-model="hobby" value="movie">
    听音乐<input type="checkbox" v-model="hobby" value="music">



//js代码
 <script type="text/javascript">
        Vue.config.productionTip = false


        new Vue({
            el:"#root",
            data:{
                account:"",
                password:"",
                sex:"",
                hobby:[],//设置为数组形式即可
            }  
        })

    </script>


```



重点：使用v-model.lazy可以实现当输入框失去焦点时，才收集输入框中用户输入的内容。




使用Vue表单提交内容案例代码：

```
<body>

    <!--准备好一个容器--> 

    <div id="root">
        <form @submit.prevent="demo">
        账号：<input id="demo" type="text" placeholder="请输入账号：" v-model.trim="account">

    </br>

        密码：<input type="password" v-model="password">

    </br>

    性别：
    男：<input type="radio" name="sex" value="male" v-model="sex">
    女：<input type="radio" name="sex" value="female" v-model="sex">


    </br>
    年龄：
    <input type="number" name="age" v-model.number="age">

    </br>

    爱好：
    学习<input type="checkbox" v-model="hobby" value="study">
    看电影<input type="checkbox" v-model="hobby" value="movie">
    听音乐<input type="checkbox" v-model="hobby" value="music">

    </br>

    所属校区：
    <select v-model="city">
        <option value="">请选择校区：</option>
        <option value="beijing">北京</option>
        <option value="shanghai">上海</option>
        <option value="shenzhen">深圳</option>
        <option value="wuhan">武汉</option>

    </select>

    </br>

    其他信息：
    <textarea v-model.lazy="other">


    </textarea></br>


    <input type="checkbox" v-model="agree">阅读并同意遵守<a href="www.baidu.com">《用户协议》</a></br>

    <button>点击提交</button>

            
        </form>
        
    </div>




    <script type="text/javascript">
        Vue.config.productionTip = false


        new Vue({
            el:"#root",
            data:{
                account:"",
                password:"",
                sex:"",
                hobby:[],
                city:"",
                other:"",
                agree:"",
                age:18,

            },
            methods:{
                demo(){
                    // console.log(JSON.stringify(this._data))
                }
            }
        })

    </script>
</body>


```



#### 039-过滤器



重点：多个组件之间的过滤器配置不能够共享。



Vue中配置全局过滤器的方法：
```

 //配置全局过滤器
        Vue.filter('mySlice',function(value){
            return value.slice(0,4)
        })


```

Vue中的过滤器在页面中有两种配置方法：
1.插值语法实现：
```

//配置全局过滤器
        Vue.filter('mySlice',function(value){
            return value.slice(0,4)
        })

//页面代码：
<h2>过滤器中设置只显示年份</h2>
        {{time | timeFilters | mySlice}}

```

2.v-bind实现过滤器的配置：
```
//配置全局过滤器
        Vue.filter('mySlice',function(value){
            return value.slice(0,4)
        })



//前端html页面中代码

<h2>测试过滤器的v-bind用法</h2>
        <input type="text" :x = "msg | mySlice">

```



过滤器总结：
1.定义：对要显示的数据进行特定格式化后再显示（适用于一些简单的逻辑处理）
2.语法：
    （1）注册过滤器：Vue.filter(name, callback) 或 new Vue(filters:{})\
    （2）使用过滤器：{{xxx | 过滤器名}} 或 v-bind:属性 = "xxx | 过滤器名"
    备注：
    （1）过滤器也可以接收额外的参数，多个过滤器也可以实现串联
    （2）并没有改变技术的数据，是产生新的对应的数据



#### 040-v-text指令

重点：v-text会拿到name的值，然后替换掉div中原来的数据。
```
 <div v-text="name"></div>

```


注意：v-text会将其中的数据都当做正常的文本进行解析，尽管其中含有很多`<h2>`等页面标签，它不会解析页面标签。


v-text作用：向其所在的标签中渲染data中的数据内容。


//截止到2022.8.16晚上22：56为止







