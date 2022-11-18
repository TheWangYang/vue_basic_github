## 尚硅谷Vue系列教程学习笔记（9）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)




#### 081-组件自定义事件-解绑


解绑事件案例代码：

1.适用于解绑一个事件的代码：
```

 unbind() {
      // eslint-disable-next-line vue/no-deprecated-events-api
      this.$off("addData");//解绑事件
    },


```


2.适用于解绑多个事件的案例代码：

```

 //解绑多个事件
      // eslint-disable-next-line vue/no-deprecated-events-api
      this.$off(["addData", "demo"]);


```

3.适用于所有的事件解绑案例代码：

```
//解绑所有事件
      // eslint-disable-next-line vue/no-deprecated-events-api
      this.$off();

```


总结：一个组件被销毁了，那么其身上的自定义事件就不奏效了。



#### 082-组件自定义事件-总结

注意：当使用ref的方法在App.vue组件中给Student.vue组件设置addData函数时，后边跟的function(){}中的this对象表示的Student.vue组件的vc实例对象

```

//直接将上述的this.getStudentName函数语句写到function(){}中
    this.$refs.student.$on("addData", function(){
      console.log(this)//注意：此处的this为Student的vc对象而不是App.vue组件的vc对象
    })


```

解释：上述现象出现的原因是Vue中规定了谁触发了addData事件，那么这个函数中的this指的就是谁。



注意：上述写法也是可以实现功能的，只需将后边的function()函数写为箭头函数即可。



重点：为了给组件设置原生事件，需要在App.vue组件中设置如下格式：


```

//使用native选项即可：
<Student @addData="getStudentName" @click.native="testClick"/>


```

注意：上述写法已经在Vue version2中弃用。



#### 083-TodoList-自定义事件


重点：需要注意的是通过组件传值的方式传递的数据不可以使用this.$emit()进行调用。



#### 084-全局事件总线1


重点：全局事件总线的作用：可以实现任意组件之间的通信。


需要设置一个组件，其中包含了其他很多组件需要调用的函数，这个组件需要满足以下条件：
1.所有组件都可以看见它；
2.该组件可以调用$on/$off/$emit函数；

实现方法：在main.js中引入Vue.prototype.xxx即可：
```

//引入prototype
Vue.prototype.x = {a:1, b:2}

//然后在xxx.vue组件中调用即可
mounted(){
      console.log(this.x)
}

```


#### 085-全局事件总线2

1.第一种使用Vue.prototype.xxx实现全局事件总线通信方法案例代码：


```
//在main.js中添加如下代码：
//将新创建的vc实例对象赋值给Vue.prototype
const Demo = Vue.extend({})
const demo = new Demo()
Vue.prototype.x = demo


//在School.vue组件中的mounted()方法中添加如下代码：
 mounted(){
        // console.log(this.x)
        //在父组件中给prototype中的x对象设置一个on事件
        this.x.$on("getStudentName",(studentName)=>{
            console.log("School组件得到了学生姓名为：",studentName)
        })
    }



//在Student.vue组件中的methods:{}对象中添加如下代码：
 methods:{
    sendNameToSchool(){
        //在子组件中给prototype中的x对象设置一个emit事件，用来调用School组件传递进去的on事件
        this.x.$emit("getStudentName","wyy")
    }
  }


```

解析：使用上述代码，即可完成Student.vue子组件向School.vue父组件中传入wyy字符串的studentName数据；


2.第二种，使用Vue.prototype方法实现组件间通信的方法：

```
new Vue({
    el: "#app",
    render: h => h(App),
    beforeCreate() {
        Vue.prototype.$bus = this
    }

})

```


编码建议：
1.可以在src文件夹下面创建一个constans.js文件，里面创建一个const对象。
2.建议在组件消失之前取消绑定的事件。

```

//需要在组件的beforeDestroy()函数中，调用如下代码：
beforeDestory(){
    this.$bus.$off('xxxx')
}

```


#### 086-TodoList案例-使用事件总线

需要修改的地方：
1.main.js中在newVue的生命周期函数beforeCreate()函数中安装全局事件：
```

Vue.prototype.bus = this


```


2.删除App.vue组件中向子组件传入函数调用的代码，并删除子组件中接受的props属性中对应的参数‘



3.在App.vue的newVue中的mounted()函数中给bus事件总线设置函数：
```


  //挂载钩子
  mounted:{
    this.$bus.on('checkTodo',this.checkTodo)
    this.$bus.on('deleteTodo',this.deleteTodo)
    this.$bus.on('checkAllTodo',this.checkAllTodo)
    this.$bus.on('clearAll',this.clearAll)
}

```

4.在子组件中使用emit函数调用在App.vue组件中设置的函数：
```
 changeCheck(id){
      //向App.vue组件中的checkTodo函数传入参数id
      // this.checkTodo(id)
      this.$bus.$emit("checkTodo",id)
      console.log(id)
    },
    ...

```



#### 087-消息订阅与发布

重点：消息订阅与发布也是实现组件间通信的方法：


消息订阅的步骤：
1.订阅消息：消息名；
2.发布消息：消息内容；


引入pubsub.js之后可以在任意一个框架里面订阅和发布消息。

实现代码如下：

```
//在接收方中安装pubsub-js库
//在挂载函数mounted中订阅消息
mounted() {
    this.pubId = pubsub.subscribe("demo", function (magName, data) {
      console.log("demo", data);
    });
  },
  //在该组件销毁之前，取消消息订阅
  beforeUnmount() {
    pubsub.unsubscribe(this.pubId);
  }


//在发送方中安装pubsub-js库
//在methods属性中发送消息
methods:{
    sendNameToSchool(){
        //在子组件中给prototype中的x对象设置一个emit事件，用来调用School组件传递进去的on事件
        // this.$bus.$emit("getStudentName","wyy")
        // console.log(this.name)
        pubsub.publish("demo",666)
    }
  }

```



#### 088-TodoList案例-使用消息订阅和接收方法实现

实现案例代码：
```

//在App.vue中写入：
// 挂载钩子
  mounted(){
    //使用消息订阅与发布的方法
    this.deleteTodoId = pubsub.subscribe("deleteTodo",(msgName, id)=>{
      console.log("deleteTodo",msgName, id)
      //调用删除todo事务的函数，向其中传入参数id
      this.deleteTodo(id)
  })
  },


//在MyItem.vue组件中写入：
 //点击删除按钮，得到对应的id
    deleteWork(id){
      //增加危险操作的选项
      if(confirm("确定删除该项任务么？")){
        pubsub.publish("deleteTodo", id)
      }
    }
```



#### 089-TodoList案例-编辑

实现功能描述：在MyItem.vue组件中设置一个编辑按钮，点击之后，将该todo项从文本改为包含当前文本值的input输入框，然后修改之后，鼠标从input框中消失之后，input框变为普通文本。



实现代码：

```
//在MyItem.vue组件中的代码：
//在html页面中的代码
<button class="btn btn-edit" @click="handleEdit(todo)">编辑</button>


//在script脚本中的代码
//---------------------------设置处理编辑逻辑的按钮逻辑-------------------------
    handleEdit(todo){
      if(todo.hasOwnProporty('isEdit')){
        todo.isEdit = true
      }else{
        // todo.isEdit = true
        this.$set(todo, "isEdit", true)//需要使用$set的方式给todo对象增加一个响应式的属性isEdit
      }
    },
    //失去焦点的时候，修改todo对象
    handleBlur(todo,event){
      todo.isEdit = false
      this.$bus.$emit("updateTodo", todo.id, event.target.value)
    },



//在App.vue组件中的代码：
methods:{
     //更改todo对象函数
    updateTodo(id,name){
        this.todos.forEach((todo) => {
            if(todo.id === id){
                todo.name = name   //将对应的值取反
            }
        })
    },
}
 // 挂载钩子
  mounted(){
    //给bus绑定updateTodo
    this.$bus.$on('updateTodo',this.updateTodo)
  },

```


#### 090-$nextTrick

功能需求：当点击todo列表中的编辑按钮后，焦点也随之来到input输入框中。

重点：$nextTick会在dom结点更新之后再执行。

实现代码：

```
//需要在MyItem.vue组件中增加$nextTick的使用：
 handleEdit(todo){
      if(todo.hasOwnProporty('isEdit')){
        todo.isEdit = true
      }else{
        // todo.isEdit = true
        this.$set(todo, "isEdit", true)//需要使用$set的方式给todo对象增加一个响应式的属性isEdit
      }

      //将焦点设置到input框中
      this.$nextTick(function(){
        //input输入框获得焦点
        this.$refs.inputName.focus()
      })
    },

```


//截止到2022年8月29日上午11：39























