## 尚硅谷Vue系列教程学习笔记（10）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)




#### 091-动画效果
重点：Vue中给标签设置动画效果的方法，将该标签设置进入trasition标签中。


案例代码：
```

<template>
  <div>
    <button @click="change">显示或隐藏</button>

    <transition>
         <h1 v-show="isShow">你好！</h1>
    </transition>
   
  </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Test",
    data(){
        return {
            isShow:true
        }
    },
    methods:{
        change(){
            console.log("控制显示或隐藏调用成功...")
            this.isShow = !this.isShow
        }
    }
}
</script>

<style>

h1{
    background-color: skyblue;
}


/*使用Vue中自带的动画写法*/
.v-enter-active{
    animation: donghuaDemo 1s;
}

.v-leave-active{
    animation: donghuaDemo 1s reverse;
}


/* .come{
    animation: donghuaDemo 1s;
} */

/* .go{
    animation: donghuaDemo 1s reverse;
} */


@keyframes donghuaDemo {
    from{
        transform: translateX(-100%);
    }    

    to{
        transform: translateX(0px);
    }
}


</style>

```

同时，需要注意的是，还可以给每个动画取名字name，但是此时style中写的xxx-enter-actvie和xxx-leave-active。
案例代码：
```
<template>
  <div>
    <button @click="change">显示或隐藏</button>

    <transition name="hello">
         <h1 v-show="isShow">你好！</h1>
    </transition>
   
  </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Test",
    data(){
        return {
            isShow:true
        }
    },
    methods:{
        change(){
            console.log("控制显示或隐藏调用成功...")
            this.isShow = !this.isShow
        }
    }
}
</script>

<style>

h1{
    background-color: skyblue;
}


/*使用Vue中自带的动画写法*/
.hello-enter-active{
    animation: donghuaDemo 1s;
}

.hello-leave-active{
    animation: donghuaDemo 1s reverse;
}


/* .come{
    animation: donghuaDemo 1s;
} */

/* .go{
    animation: donghuaDemo 1s reverse;
} */


@keyframes donghuaDemo {
    from{
        transform: translateX(-100%);
    }    

    to{
        transform: translateX(0px);
    }
}


</style>


```

重点：在transition标签中添加`:appear="true"`即可实现加载页面时即出现动画效果。


注意：上述的transition标签在解析的时候直接被删掉了。


#### 092-过渡效果

使用过渡效果实现显示或隐藏动画的案例代码：

```
<template>
  <div>
    <button @click="change">显示或隐藏</button>

    <transition name="hello">
         <h1 v-show="isShow">你好！</h1>
    </transition>
   
  </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Test",
    data(){
        return {
            isShow:true
        }
    },
    methods:{
        change(){
            console.log("Test2中控制显示或隐藏调用成功...")
            this.isShow = !this.isShow
        }
    }
}
</script>

<style scoped>

h1{
    background-color: skyblue;
    transition: 0.5s linear;
}

/*使用Vue中自带的动画写法2*/
/*进入的起点*/
.hello-enter{
    transform: translateX(-100%);
}

/*进入的终点*/
.hello-enter-to{
    transform: translateX(0);
}

/*离开的起点*/
.hello-leave{
    transform: translateX(0);
}

/*离开的终点*/
.hello-leave-to{
    transform: translateX(-100%);
}

</style>



//上述样式中简写形式为：

/*使用Vue中自带的动画写法2*/
/*进入的起点, 离开的终点*/
.hello-enter, .hello-leave-to{
    transform: translateX(-100%);
}

/*进入的终点, 离开的起点*/
.hello-enter-to, .hello-leave{
    transform: translateX(0);
}


/*将切换效果设置到xxx-enter-active和xxx-leave-active中*/
.hello-enter-actrive, .hello-leave-active{
    transition: 0.5s linear;
}
```



#### 093-多个元素过渡效果实现

注意：transition标签仅仅可以被使用给一个单独的标签元素中。


多个元素过渡效果实现使用transition-group的代码：

```
<template>
  <div>
    <button @click="change">显示或隐藏</button>

    <transition-group name="demo" appear>
         <h1 v-show="isShow" key="1">你好！</h1>
         <h1 v-show="isShow" key="2">你好！</h1>
         <h1 v-show="isShow" key="3">你好！</h1>
         <h1 v-show="isShow" key="4">你好！</h1>
    </transition-group>
   
  </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Test",
    data(){
        return {
            isShow:true
        }
    },
    methods:{
        change(){
            console.log("Test2中控制显示或隐藏调用成功...")
            this.isShow = !this.isShow
        }
    }
}
</script>

<style scoped>

h1{
    background-color: skyblue;
}


/*使用Vue中自带的动画写法1*/
.demo-enter-active{
    animation: test3Donghua 0.5s;
}

.demo-leave-active{
    animation: test3Donghua 0.5s reverse;
}

@keyframes test3Donghua {
    from{
        transform: translateX(-100%);
    }    

    to{
        transform: translateX(0px);
    }
}

</style>


```


#### 094-集成第三方动画


重点：使用方法（以Animate.css库为例）案例代码：

```
<template>
  <div>
    <button @click="change">显示或隐藏</button>

    <transition-group 
    name="animate__animated animate__   bounce" 
    enter-active-class="animate__swing"
    leave-active-class="animate__backOutUp"
    appear>
         <h1 v-show="isShow" key="1">你好！</h1>
         <h1 v-show="isShow" key="2">你好！</h1>
         <h1 v-show="isShow" key="3">你好！</h1>
         <h1 v-show="isShow" key="4">你好！</h1>
    </transition-group>
   
  </div>
</template>

<script>
//引入animate.css样式库
import 'animate.css'
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Test",
    data(){
        return {
            isShow:true
        }
    },
    methods:{
        change(){
            console.log("Test2中控制显示或隐藏调用成功...")
            this.isShow = !this.isShow
        }
    }
}
</script>

<style scoped>

h1{
    background-color: skyblue;
}

</style>


```


#### 095-总结动画与过渡（略）



#### 096-Vue配置代理-方式1

目的：解决ajax请求跨域的问题。

常见的发送一个ajax请求的方法：
1.xhr（不常用的方法）
2.jQuery（底层封装了xhr）：$.get $.post请求等；在Vue/React项目中很少使用jQuery发送请求，因为jQuery库主要是操作DOM；
3.axios（底层封装了xhr，推荐使用）；
4.fetch（和xhr是平级的）：不推荐使用，因为兼容性较差；



解决跨域问题的方法：
1.使用cors解决跨域；
2.使用jsonp解决跨域，不常用的最主要原因是只能解决get请求问题；
3.代理服务器（开发中间用的最多），理解：代理服务器和服务器之间的通信是http协议，且不受浏览器同源限制；



使用代理服务器的方法：
1.使用nginx实现；
2.vue-cli脚手架实现；


重点：public文件夹里面有的资源，都算是8080服务器中有的。


使用Vue中的代理服务器存在的不足：
1.8080服务器中有的，会直接返回给请求，而不会请求其他服务器；
2.使用该种方式配置代理服务器时，不能配置多代理；


使用vue-cli脚手架实现代理服务器的代码：

```

//在Vue.config.js文件中配置如下代码：

const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  //开启代理服务器
  devServer: {
    proxy:"http://localhost:5000"
  }
})





//在App.vue组件中使用如下代码：
    <template>
  <div id="root">
    <button @click="getStudents">获取学生信息</button>
  </div>
</template>
    <script>
    //引入axios
    import axios from 'axios'

export default {
  name: "App",
  methods:{
    getStudents(){
      axios.get('http://localhost:8080/students').then(
        response => {console.log("请求成功了", response.data)},
        error => {console.log("请求失败了", error.message)}
      )
    }
  }
};
</script>

    <style>
body {
  background: #fff;
} 
</style>

```


上述代码即可解决浏览器的同源限制问题。



#### 097-Vue配置代理-方式2

使用Vue自带的代理服务器请求目标服务器方法案例代码：

```
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  //开启代理服务器（方式1）
  // devServer: {
  //   proxy:"http://localhost:5000"
  // }

  //配置代理服务器（方式2）
  devServer: {
    proxy: {
      '/demo': {
        target: 'http://localhost:5000',
        pathRewrite: {//重写请求路径，将前缀/demo替换为空字符串
          '^/demo':''
        },
        ws: true,//用于支持webSocket
        changeOrigin: true//用于在代理服务器和目标服务器之间决定代理服务器是否说谎，称自己是否和目标服务器端口号一致。
      }
    }
  }

})

```


总结：
1.上述的/api表示，如果想走Vue配置的代理红服务器，需要在请求前边加上api前缀；
2.上述的target表示，请求的目标服务器地址；
3.添加pathRewrite属性即可将前缀去掉，然后即可实现对服务器的访问；



#### 098-静态组件

需要实现的功能需求；
用户输入想要寻找的github用户名，然后点击按钮，即可将服务器返回来的用户头像和用户名展示到List组件中。


引入官方样式的方法：
1.引入其他官方的样式时，如果官方的样式中存在对没有的资源的引入，那么在App.vue组件中使用import引入时会出现错误警告。
2.在public文件夹中建立一个css文件夹，然后将官方的样式放入到其中，并在index.html文件中引入即可。



案例实现代码：

```

//在App.vue中的代码
    <template>
  <div id="root">
    <div class="container">
    <Search/>
    <List/>
    </div>
  </div>
</template>
    <script>

//引入组件
import Search from "./components/Search.vue";
import List from "./components/List.vue";

export default {
  name: "App",
  components: {
    Search,
    List,
  },
};
</script>


//在Search.vue组件中代码
<template>
   <section class="jumbotron">
        <h3 class="jumbotron-heading">Search Github Users</h3>
        <div>
          <input
            type="text"
            placeholder="enter the name you search"
          />&nbsp;<button>Search</button>
        </div>
      </section>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Search",
    methods:{
        SearchClick(){
            console.log('SearchClick...')
        }
    },
    
}
</script>

<style>

</style>


//在List.vue组件中代码
<template>
  <div class="row">
    <div class="card">
      <a href="https://github.com/xxxxxx" target="_blank">
        <img src="https://cn.vuejs.org/images/logo.svg" style="width: 100px" />
      </a>
      <p class="card-text">xxxxxx</p>
    </div>
    <div class="card">
      <a href="https://github.com/xxxxxx" target="_blank">
        <img src="https://cn.vuejs.org/images/logo.svg" style="width: 100px" />
      </a>
      <p class="card-text">xxxxxx</p>
    </div>
    <div class="card">
      <a href="https://github.com/xxxxxx" target="_blank">
        <img src="https://cn.vuejs.org/images/logo.svg" style="width: 100px" />
      </a>
      <p class="card-text">xxxxxx</p>
    </div>
    <div class="card">
      <a href="https://github.com/xxxxxx" target="_blank">
        <img src="https://cn.vuejs.org/images/logo.svg" style="width: 100px" />
      </a>
      <p class="card-text">xxxxxx</p>
    </div>
    <div class="card">
      <a href="https://github.com/xxxxxx" target="_blank">
        <img src="https://cn.vuejs.org/images/logo.svg" style="width: 100px" />
      </a>
      <p class="card-text">xxxxxx</p>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "List",
};
</script>

<style scoped>
.album {
  min-height: 50rem;
  padding-top: 3rem;
  padding-bottom: 3rem;
  background-color: #f7f7f7;
}

.card {
  float: left;
  width: 33.3333%;
  padding: 0.78rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: 0.75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}

</style>


```


#### 099-列表展示

github用户头像/用户名等信息的列表展示案例代码：

```
//使用pubsub实现组件间的通信
//Search.vue组件中的代码
<template>
   <section class="jumbotron">
        <h3 class="jumbotron-heading">Search Github Users</h3>
        <div>
          <input
            type="text"
            placeholder="enter the name you search"
            v-model="keyWords"
          />&nbsp;<button @click="SearchClick">Search</button>
        </div>
      </section>
</template>

<script>
//引入axios
import axios from 'axios'
//采用消息订阅的方式在组件之间传送数据
import pubsub from 'pubsub-js'
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Search",
    methods:{
        SearchClick(){
            //使用axios发送get请求，并使用，模板字符串解析
            axios.get(`https://api.github.com/search/users?q=${this.keyWords}`).then(
                response => {
                    console.log("请求成功，得到数据为：",response.data.items)
                    //发送消息
                    pubsub.publish("userItems", response.data.items)

                },
                error => {
                    console.log("请求失败了，得到的错误信息为：",error.message)
                }
            )
            console.log('SearchClick...')
        }
    },
    data(){
        return {
            keyWords:""
        }
    }
    
}
</script>

<style>

</style>



//List组件中代码
<template>
  <div class="row">
    <div class="card" v-for="user in users" :key="user.login">
      <a :href="user.html_url" target="_blank">
        <img :src="user.avatar_url" style="width: 100px" />
      </a>
      <p class="card-text">{{user.login}}}}</p>
    </div>
  </div>
</template>

<script>
//引入pubsub-js组件
import pubsub from "pubsub-js";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "List",
  data() {
    return {
      users: [],
    };
  },
  //设置挂载的时候，订阅消息
  mounted() {
    //获得接收消息的pubId，便于在该组件销毁之前取消订阅消息
    this.pubId = pubsub.subscribe("userItems", (msgName, users) => {
      console.log("List组件中得到了用户数据：", users);
      this.users = users
    });
  },
  //
  beforeUnmount() {
    pubsub.unsubscribe(this.pubId);
    console.log("取消了List组件的消息订阅...");
  },
};
</script>

<style scoped>
...
</style>


```



#### 0100-github案例-完善案例

上述github案例完善后代码如下。

重点：将页面组件中的状态定义为一个statusObj对象。

```

//Search.vue组件中的代码
<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="keyWords"
      />&nbsp;<button @click="SearchClick">Search</button>
    </div>
  </section>
</template>

<script>
//引入axios
import axios from "axios";
//采用消息订阅的方式在组件之间传送数据
import pubsub from "pubsub-js";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Search",
  data() {
    return {
      keyWords: "",
    };
  },
  methods: {
    SearchClick() {
      //定义一个状态对象
      let statusObj = {
        isFirst: false,
        isLoading: true,
        errMsg: "",
        users: [],
      };

      //请求前
      pubsub.publish("getListData", statusObj);
      console.log("请求前loading状态...");

      //请求中
      //使用axios发送get请求，并使用，模板字符串解析
      axios.get(`https://api.github.com/search/users?q=${this.keyWords}`).then(
        (response) => {
          //更新数据对象中的属性值
          statusObj.isLoading = false;
          statusObj.errMsg = "";
          statusObj.users = response.data.items;

          //发送消息
          pubsub.publish("getListData", statusObj);
          console.log("请求成功状态...");
        },
        (error) => {
          //更新状态对象
          statusObj.errMsg = error.message;
          statusObj.isLoading = false;

          pubsub.publishs("getListData", statusObj);
          console.log("请求失败状态...");
        }
      );
      console.log("SearchClick...");
    },
  },
};
</script>

<style>
</style>



//List.vue组件中的代码
<template>
  <div class="row">
    <!--展示用户列表与否取决于api返回的数组长度-->
    <div
      v-show="userInfo.users.length"
      class="card"
      v-for="user in userInfo.users"
      :key="user.login"
    >
      <a :href="user.html_url" target="_blank">
        <img :src="user.avatar_url" style="width: 100px" />
      </a>
      <p class="card-text">{{ user.login }}</p>
    </div>
    <!--展示欢迎词-->
    <div v-show="userInfo.isFirst">
      <h1>Welcome to the Search GitHub!</h1>
    </div>

    <!--展示加载中-->
    <div v-show="userInfo.isLoading">
      <h1>Welcome to the Search GitHub!</h1>
    </div>
    <!--展示错误信息-->
    <div v-show="userInfo.errMsg">
      <h1>{{ userInfo.errMsg }}</h1>
    </div>
  </div>
</template>

<script>
//引入pubsub-js组件
import pubsub from "pubsub-js";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "List",
  data() {
    return {
      deep:true,
      userInfo: {
        //增加几个额外的状态信息
        isFirst: true, //是否为首次请求api
        isLoading: false, //是否在加载中
        errMsg: "", //请求错误时返回的错误信息
        users: [],
      }
    };
  },
  //设置挂载的时候，订阅消息
  mounted() {
    //获得接收消息的pubId，便于在该组件销毁之前取消订阅消息
    this.pubId = pubsub.subscribe("getListData", (msgName, statusObj) => {
      console.log("List组件收到了用户更新的数据为：", statusObj);
      this.userInfo = {...this.userInfo, ...this.statusObj};
    });
  },
  //
  beforeUnmount() {
    pubsub.unsubscribe(this.pubId);
    console.log("取消了List组件的消息订阅...");
  },
};
</script>

<style scoped>
...
</style>


```


//截止到2022.8.30下午18：04止



