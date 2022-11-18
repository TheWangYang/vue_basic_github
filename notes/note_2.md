## 尚硅谷Vue系列教程学习笔记（2）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。



### 011-数据代理（主要是熟悉vue中的方法）


```

let person = {
    name:'张三',
    sex:'男',
    age:18  //直接在原对象中添加age属性，是可以枚举的
}

//使用如下代码时，表示对象中的age属性是不可以被枚举的
 Object.defineProperty(person,'age',{
            value:18,
            enumerable
})


Object.keys(person) //表示列举出来person对象中的所有属性


//如果想在使用Object.defineproperty()时，使得新定义的属性可以被枚举，那么在上述代码中添加属性
enumerable即可

```


下面为Object.defineproperty()函数中的配置项

```
 Object.defineProperty(person,'age',{
            value:18,
            enumerable:true, //控制属性是否可以被枚举，默认为false
            writable:true, //控制属性是否都可以被修改，默认为false
            configurable:true //控制属性是否可以被删除，默认为false
        })


```


在Object.defineproperty()函数中，还有get()和set()方法，当新定义的属性值更换时，set()方法会被调用，
当定义的属性值被获得时，get()方法会被调用。


```

Object.defineProperty(person, 'age', {
            // value:18,
            // enumerable:true, //控制属性是否可以被枚举，默认为false
            // writable:true, //控制属性是否都可以被修改，默认为false
            // configurable:true //控制属性是否可以被删除，默认为false


            //当有人读取age属性时，get函数就会被调用，且返回值就是age的值
             get() {
                console.log("有人读取了age属性")
                return number

            },
            
            //当有人修改age属性时，set函数就会被调用，且会收到修改的具体值
            set(value) {
                console.log("有人修改了age属性且值为：", value)

                number = value
            }

        })


```



#### 012-理解数据代理


```

 <script>
        let obj1 = {x:100}
        let obj2 = {y:200}

        //实现在obj2中修改obj1中的属性x的值的代码
        Object.defineProperty(obj2,'x',{
            get(){
                return obj1.x
            },

            set(value){
                obj2.x = value

            }
        })
    </script>


```

上述代码实现了：在obj2对象中得到obj1对象的x属性的值，且在obj2的x属性修改时，obj1中的属性x会发生改变。



#### 013-Vue中的数据代理


```


const vm = new Vue({
            el:'#root',
            data:{
                name:'xjtu',
                address:'西安'
            }

        })


```
上述代码中，vm对象对标签中{{name}}和{{address}}的数据绑定使用了
Object.defineproperty()函数，在view组件部分得到name和address的值时需要调用getter方法，
而当设置name和address时需要调用setter方法。


通过vm读name属性的值，是读的data.name，通过vm读address属性的值，是读的data.address，这就是数据代理。


```

 const vm = new Vue({
            el:'#root',
            data:{
                name:'xjtu',
                address:'西安'
            }

        })

//上述代码中的vm实例对象将data数据保存其中，然后使用vm._data就可以得到属性了。

```

验证vm对象调用setter方法对data属性设置值之后，data中的数据是否也改变了。
代码如下：

```
 <script type="text/javascript">
        Vue.config.productionTip = false

        let data = {
            name: 'xjtu',
            address: '西安'
        }

        const vm = new Vue({
            el:'#root',
            data
        })


    </script>


//然后在浏览器的命令行中输入：
vm._data === data

-> true

//输出为true即验证。

```


vm中的_data可以直接在模板中直接使用：
```

<h1>学校名称：{{_data.name}}</h1>
<h1>学校地址：{{_data.address}}</h1>


```


实际上，数据代理做了：将data里面的数据放入到vm对象实例中一份。
目的：为了方便编码。
实现：使用了Object.defineproperty()函数。




做一个总结：
1.Vue中的数据代理：通过vm对象来代理data对象中属性的操作。
2.Vue中数据代理的好处：更加方便的操作data中的数据
3.基本原理：通过Object.defineproperty()函数将data中的所有属性添加到vm上，为每一个添加到vm上的属性都指定了getter/setter方法，在getter/setter方法中对data中的属性进行操作。


vm需要实现对data中属性的监听从而可以在data中属性发生改变时，反映到页面组件标签中。



#### 014-事件处理


```

 <button v-on:click="showInfo">点我显示提示信息</button>

<script type="text/javascript">
        Vue.config.productionTip = false


        function showInfo() {
            alert('同学你好')
        }

        new Vue({
            el: '#root',
            data: {
                name: "xjtu"
            }

        })

    </script>

//上述button中的v-on:click属性，只能使用vm事例中的属性，无法使用js中其他非vm实例中的属性/函数等，因此需要将上述的showInfo放入到new Vue()中。
 


```



如果想实现上述点击button之后，页面中出现提示框，那么需要设置如下代码：

```


new Vue({
            el: '#root',
            data: {
                name: "xjtu"
            },
            methods:{
                showInfo(){
                    alert('同学你好！')
                }
            }

        })



```
上述代码中，将showInfo方法放入vm对象实例methods属性中即可。




重点：所有被vue对象管理的函数最好写成普通函数，此时打印出来的this为vm对象实例。

```

showInfo(){
                    alert('同学你好！')
                    console.log(this)  //此处的this为vm对象实例或组价实例对象
                }


```

如果写成如下格式，那么打印出来的this为window对象

```

showInfo:(event)=>{
                    alert('同学你好！')
                    console.log(this)  //此处的this为window对象
                }


```


v-on:可以简写为@。



在函数中传参的代码：


```

<button @click="showInfo1">点我显示提示信息1</button>
<button @click="showInfo2(66)">点我显示提示信息2</button>


//想传参需要在click后边的函数后添加一个()，不传参的话，不需要添加。
```


但是上述代码在传参之后，会导致事件的event消失，那么可以采用如下代码；

```

<button @click="showInfo2(66,$event)">点我显示提示信息2</button>

//即采用$event关键字（占位符）。

//那么在js中接收event和number的代码如下：
 showInfo2(number,event){
                    console.log(number)
                    //alert("同学你好！")

                }
//即在showInfo2()函数中第一个参数为number，第二个为event

```

上述设置的两个click点击事件，不需要做数据代理，因为方法写完之后，等着调用就可以了。



只有配置在vm实例中的data的属性才会做数据劫持和数据代理。


上述两个showInfox()方法也是可以放入到data中的，但是很麻烦，给vue造成不必要的资源浪费。



#### 015-Vue中的事件修饰符的使用

```


 <a href="www.baidu.com" @click="showInfo">点我提示信息</a>



//js中代码：

new Vue({
            el: '#root',
            data: {
                name: "xjtu"
            },
            methods: {
                showInfo(event){
                    alert("test")
                    //组织a标签默认的跳转动作
                    event.preventDefault() //设置该代码可以阻止a标签默认跳转
                }
            }

        })


```


同时，也可以在html标签中设置如下代码来阻止a标签默认跳转：

```

<a href="www.baidu.com" @click.prevent="showInfo">点我提示信息</a>


```


Vue共提供了6个事件修饰符：
1.prevent：阻止默认事件；
2.stop：阻止事件冒泡；
3.once：事件只触发一次；
4.capture：使用事件的捕获方式；
5.self：只有在event.target是当前操作的元素事才触发事件；
6.passive：事件的默认行为立即执行，无需等待事件回调执行完毕；

上述6个事件修饰符中，1，2，3是常用的。


这里对Vue中的事件修饰符-passive进行解释：
```

<!--测试passive事件，事件的默认行为立即执行，无需等待事件回调执行完毕-->
        <ul @wheel="demo" class="list">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>

        </ul>


```

scroll属性表示：监听滑动滚动条事件。


wheel属性表示：滚动鼠标滚轮时先回调执行事件中的内容，等到内容执行完毕之后，再执行动作。这种情况会导致页面有些卡顿，且需要的内容往下滚动的动作不会立即发生，那么就可以使用事件修饰符：passive来保证事件动作立刻发生而不需要等到事件调用返回结果之后再发生动作。




#### 016-键盘事件


```
<div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <input type="text" placeholder="按下回车提示输入" @keyup.enter="showInfo"/>
    </div>

//Vue中给常用的按键起了一些别名，可以跟在标签后边的key.xxx后边进行选择。

//同时，也可以使用Vue对象中的methods属性中添加逻辑实现按下特定按键来提示信息

<script type="text/javascript">
        Vue.config.productionTip = false

        new Vue({
            el: '#root',
            data: { 
                name: "xjtu"
            },
            methods:{
                showInfo(e){

                    //输出键盘按键的编码
                    // console.log(e.keyCode)
                    // if(e.keyCode !== 13){ //enter的编码为13
                    //     return
                    // }
                     console.log(e.target.value)
                   
                    
                }
            }

        })

    </script>


```


总结Vue中常用的按键编码：
1.回车->enter
2.删除->delete（捕获删除和退格键）
3.退出->etc
4.空格->space
5.换行->tab
6.上->up
7.下->down
8.左->left
9.右->right


其中需要说明的是：tab键本身有一个切换光标的功能，按下之后可以从当前位置切换走，因此不能配合keyup使用，而应当配合keydown使用：
```

<div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <input type="text" placeholder="按下回车提示输入" @keydown.tab="showInfo"/>

</div>

```


除了tab键之外还有四个按键也比较特殊：
1.ctrl
2.shift
3.meta（也叫做win键）
4.alt


上述四个键位配合：
keyup使用时，需要在按下修饰键同时按下其他键，然后释放其他键，此时才可以触发事件。
keydown使用时，直接触发事件发生。


不推荐使用下述直接使用键位码的做法，因为某些web浏览器不再支持：
```

<div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <input type="text" placeholder="按下回车提示输入" @keydown.13="showInfo"/>
    </div>


```



可以使用Vue.config.keyCodes.xxx = 键码



事件修饰符可以连着写：

```

<div class="demo" @click="showInfo">

!--阻止事件跳转-->
        <!-- <a class='demo1' href="www.baidu.com" @click.stop.prevent="showInfo">点我提示信息</a> -->
</div>

<


//如上面代码所示，其中的click.stop.prevent表示先阻止事件冒泡，然后阻止标签默认的跳转。


```


如下代码展示了按住ctrl+y才可以触发事件动作的功能：
```

<div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <input type="text" placeholder="按下回车提示输入" @keydown.ctrl.y="showInfo"/>
    </div>


```


#### 017-计算属性

```

姓名：<span>{{fullName()}}</span>


//js中代码为：

<script type="text/javascript">
        Vue.config.productionTip = false


    
        new Vue({
            el: '#root',
            data: {
                first_name: "张",
                second_name:"三",
            },

            methods:{
                fullName(){
                    return 'test'
                }
            }
        })

    </script>


//这里说明fullName()方法的返回值插入到span标签中，所以需要在{{fullName()}}中fullName后边添加()符号。

```



重点：只要data中的数据发生改变，Vue一定会解析模板，只要重新解析模板，那么vm对象实例中的methods方法会被重新调用一次。




计算属性实现姓名案例：
计算属性是通过对属性进行操作之后生成的新属性，不在data中实现，而是在computed属性中进行实现。
vm._data中只有data对象中的数据，并没有computed对象中的数据，因为在computed中的数据是vm根据data属性中的数据后续计算出来的。



```

//html页面

<div id="root">
        姓：<input type="text" placeholder="请输入姓" v-model="first_name"/> </br>
        名：<input type="text" placeholder="请输入名" v-model="second_name"/> </br>

        姓名：<span>{{fullName}}</span>
    </div>

//js代码
new Vue({
            el: '#root',
            data: {
                first_name: "张",
                second_name:"三",
            },
            computed:{
                fullName:{
                    get(){//当有人读取fullName时，调用get方法
                        console.log("测试计算属性getter方法")
                        return this.first_name + "-" + this.second_name
                    }
                }
            }
        })



```


重点：需要注意的是在html标签中调用fullName后，get方法会被调用一次，如果页面中出现相同的模板（即fullName）被调用了多次，那么Vue不会重复请求get方法，而是在缓存中进行获得fullName的值。

上述fullName中的get方法什么时候调用：
1.初次读取fullName时；
2.所依赖的数据发生变化时；



计算属性一般是显示出来的，并不需要修改。



计算属性的简写模式：

```
const vm = new Vue({
            el: '#root',
            data: {
                first_name: "张",
                second_name:"三",
            },
            computed:{
                //简写模式
                fullName(){
                    console.log("test")
                    return this.first_name + "-" + this.second_name
                }
            }
        })



```

//截止到2022.8.14晚上19:12止




