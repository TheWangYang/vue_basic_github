## 尚硅谷Vue系列教程学习笔记（8）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)



#### 071-TodoList案例-初始化列表

重点：js里面存储数据两种选择：数组/对象；

技巧：对于每个Item来说，使用数组保存多个item，然后对于每个item来说，可以记为一个对象。

绑定动态数据代码实现：

```
//将数据写入到了页面中，然后在Item组件中使用props属性接收传入的值
<template>
  <ul class="todo-main">
    <MyItem v-for="todoObj in todos" :key="todoObj.id" :todo="todoObj"/>
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

  //装载数据
  data(){
    return {
      todos:[
        {id:"0001", name:"吃饭", done:false},
        {id:"0002", name:"看电影",done:true},
        {id:"0003", name:"玩游戏",done:true},
      ]
    }
  }
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




//在MyItem.vue组件中接受数组中的对象值
<template>
  <li>
    <label>
      <input type="checkbox" :checked="todo.done"/>
      <span>{{todo.name}}</span>
    </label>
    <button class="btn btn-danger">删除</button>
  </li>
</template>

<script>
export default {
  name: "MyItem",
  //声明接受todo对象
  props:['todo']
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
            /* display: none; */
            margin-top: 3px;
        }


        li:before{
            content: initial;
        }


        li:last-child{
            border-bottom: none;
        }

</style>


```


#### 072-TodoList案例-添加数据

技巧：一个组件给父组件传数据的方法，父组件需要首先给该组件传入一个函数，该组件调用该函数，并向其中传入数据作为参数即可。


实现：MyHeader.vue组件向App.vue组件中传入数据以及用户在输入框输入数据之后，添加到list中案例代码：

```
//MyHeader.vue组件代码
<template>
  <div class="todo-header">
    <input
      type="text"
      placeholder="请输入你的任务名称，按回车键确认"
      @keyup.enter="addWork"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "MyHeader",
  methods: {
    addWork(event) {
      if (!event.target.value.trim()) {
        return alert("注意：输入不能为空！");
      }
      //(event)
      //将用户输入数据包装成一个对象
      const todoObj = {
        id: nanoid(),
        name: event.target.value,
        done: false,
      };

      //调用接收到的receiveData()方法
      this.receiveData(todoObj);

      //添加之后将输入框中的数据清空
      event.target.value = "";
    },
  },
  props: ["receiveData"],
};
</script>

<style scoped>...</style>


//App.vue组件代码
    <template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader :receiveData="receiveData"/>
        <MyList :todos="todos"/>
        <MyFooter />
      </div>
    </div>
  </div>
</template>

    <script>
//引入组件
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  components: {
    MyHeader,
    MyFooter,
    MyList,
  },

  //装载数据
  data() {
    return {
      todos: [
        { id: "0001", name: "吃饭", done: false },
        { id: "0002", name: "看电影", done: true },
        { id: "0003", name: "玩游戏", done: true },
      ],
    };
  },
  methods:{
    receiveData(todoObj){
        // console.log("我收到了Header中的数据：",todoObj)
        this.todos.unshift(todoObj)
    }
  }
};
</script>

<style>...</style>


```

技巧：数据在哪里，操作数据的方法就在哪里。

#### 073-TodoList案例-勾选动作实现


勾选实现代码：

```
//App.vue代码
//在methods中添加一个函数，实现由id得到数据，进而修改数据中的done值为其相反值
methods:{
    ...,
    //修改对应id下标的数据的done值为相反值
    checkTodo(id){
         console.log("App.vue : ",id)
        this.todos.forEach((todo) => {
            if(todo.id === id){
                // console.log("find that")
                todo.done = !todo.done   //将对应的值取反
            }
        })
    }
  }




//然后在App.vue中的MyList组件传入上述函数，在MyList.vue组件中接受上述函数之后，继续传给MyItem.vue组件，然后在MyItem.vue组件中的监听勾选与否的函数中使用上述函数，并向其中传入id参数即可。

```


#### 074-TodoList案例-删除动作


删除任务案例代码：


```

//在App.vue组件中的methods属性中添加如下方法
 //删除选定的任务
    deleteTodo(id){
      console.log("Delete todo...")
      this.todos = this.todos.filter((todo)=>{
        return todo.id !== id
      })
    }

//然后通过MyList.vue传到MyItem.vue组件中，然后在MyItem.vue组件中调用上述方法，传入参数id即可。


```



#### 075-TodoList案例-底部统计功能实现

重点：在统计数组中的已经做完的任务数量时，使用两种方法均可：

```
//第一种写法，使用for循环遍历
doneSize() {
      let sum = 0;
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].done === true) {
          sum++;
        }
      }
      return sum;
    },


//第二种方法：使用reduce函数
doneSize(){
  sum = this.todos.reduce((pre, current)=>{
        return pre + (current.done === true ? 1 : 0)
      },0)

      return sum
}
      

```


#### 076-TodoList案例-底部交互动作实现

底部全选/全不勾选的逻辑实现和清除所有已完成任务动作的代码：

```

//在App.vue组件中设置如下函数
 //全选或取消全选
    checkAllWork(done){
      this.todos.forEach((todo)=>{
        todo.done = done
      })
    },
    //删除所有已完成任务
    clearAll(){
      this.todos = this.todos.filter((todo)=>{
        return todo.done === false
      })
    }


//然后传到MyFooter.vue组件中并进行调用即可


```



#### 077-TodoList案例-总结

注意：v-model是不能绑定props属性传来的值的，因为props属性的值是固定不变的。

具体重点总结见上述总结内容。



#### 078-浏览器本地存储


重点：
1.使用localStorage对象存储数据时，key和value一定都是字符串对象；
2.只要value不是string类型的，它会主动将value的值转换为string，因此如果你传入的value为一个对象，那么转为string类型之后为[Object Object]，那么需要使用的是JSON.stringfy()函数即可。
3.sessionStorage对象和localStorage对象中的方法一致，两者不同的是：只要浏览器关闭，那么sessionStorage就会被全部清除掉；




#### 079-TodoList-本地存储


本地存储TodoList的实现代码：


```

//给todos数组设置一个监视属性
  watch: {
    //完整watch监视属性写法
    todos: {
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
        //console.log("保存todoList变量到localStorage对象成功...");
      },
      immediate: true,
      deep: true,
    }
  },

//在设置初始化todos数组的值时有一个细节需要注意：就是如果localStorage.getItem('todos')的值为null时，Vue会出现错误，因此需要进行一个简单的判断如下：
//装载数据
  data() {
    return {
      todos: localStorage.getItem('todos') !== null ? JSON.parse(localStorage.getItem('todos')) : []
    };
  },

```



#### 080-组件自定义事件-绑定

重点：自定义事件是给组件使用的。


通过自定义事件将组件的数据传给其父组件：



```
//Student.vue组件中的代码
<template>
  <div>
    <h2>学生姓名：{{ name }}</h2>
    <h2>学生年龄：{{ age }}</h2>
    <button @click="sendStudentNameToApp">点击将学生姓名传给App.vue组件</button>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Student",
  data() {
    return {
      name: "wyy",
      age: 22,
    }
  },
  methods:{
    sendStudentNameToApp(){
      console.log("sendStudentDataToApp被调用了...")
      this.$emit("addData",this.name)//调用App.vue组件中定义的函数
    }
  }
};
</script>

<style>
</style>




//App.vue组件中的代码
<template>
  <h2>{{ msg }}</h2>
  <School />
  <Student v-on:addData="demo"/>
</template>

  <script>
import School from "./components/School.vue";
import Student from "./components/Student.vue";

export default {
  name: "App",
  components: {
    School,
    Student,
  },
  data() {
    return {
      msg: "你好啊！",
    };
  },
  methods:{
    demo(name){
      console.log("App得到的学生姓名为：",name)
    }
  }
};
</script>

  <style>
</style>



```


同时，实现上述功能还有另外一种写法：

```
//在App.vue组件对象中的mounted()方法
  methods: {
    // demo(name){
    //   console.log("App得到的学生姓名为：",name)
    // }
    getStudentName(name) {
      console.log("得到学生姓名为：", name);
    },
  },
  mounted() {
    //得到了Student组件的实例对象
    this.$refs.student.$on("addData", this.getStudentName);
  },


```

这种方法相当于直接获得Student.vue组件的对象，然后设置on函数并向其中传入methods中的方法


//截止到2022.8.24下午18：08止



