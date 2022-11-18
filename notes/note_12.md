## 尚硅谷Vue系列教程学习笔记（12）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)



#### 111-getters配置项

注意：getters中可以实现一些需要共享的运算逻辑等。


实现案例代码：

```

//在store/index.js文件中的代码如下：
getters: {
        //用于处理sum放大十倍之后的逻辑
        bigSum(state) {
            return state.sum * 10
        }
    },

//在组件中直接使用模板即可得到
<h3>当前求和放大十倍之后为：{{$store.getters.bigSum}}</h3>


```


#### 112-mapState和mapGetters

下面代码含义为：将mapState对象中的key:value全部展开放入到computed对象中。

```

 //计算属性
  computed:{
    //前边表示计算属性对象中的key值，后边加''的变量为state中本身存在的数据对象
    ...mapState({he:'sum',xuexiao:'school',xueke:"subject"})
  },


```

总结：上述的mapState说白了就是帮你生成代码用的，即借助mapState生成计算属性，上述代码为mapState的对象写法。


mapState的数组写法：

```
computed:{
    //前边表示计算属性对象中的key值，后边加''的变量为state中本身存在的数据对象
    ...mapState(['sum','school','subject'])
  },


```


mapGetters写法：
```
//计算属性
  computed:{
    ...mapState({he:'sum',xuexiao:'school',xueke:"subject"}),
    ...mapGetters(['bigSum'])
  },

```


#### 113-mapActions和mapMutations

mapActions案例代码：

```
//Count.vue组件中代码
<template>
  <div>
    <h2>当前求和为：{{ he }}</h2>
    <h3>当前求和放大十倍之后为：{{ bigSum }}</h3>
    <h3>我在{{ xuexiao }}学习{{ xueke }}</h3>
    <select v-model.number="curr">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
      <option value="5">5</option>
      <option value="6">6</option>
      <option value="7">7</option>
      <option value="8">8</option>
      <option value="8">9</option>
      <option value="10">10</option>
    </select>

    <button @click="increment(curr)">+</button>
    <button @click="decrement(curr)">-</button>
    <button @click="incrementOdd(curr)">当前求和为奇数时再加</button>
    <button @click="incrementWait(curr)">等一等再加</button>
  </div>
</template>

<script>
//引入mapState
import { mapGetters, mapState, mapMutations, mapActions} from "vuex";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Count",
  data() {
    return {
      curr: 1,
    };
  },
  //计算属性
  computed: {
    ...mapState({ he: "sum", xuexiao: "school", xueke: "subject" }),
    ...mapGetters(["bigSum"]),
  },

  methods: {
    //使用mapMutations实现
    ...mapMutations({
      //不需要执行actions逻辑的函数可以直接使用mapMutations进行操作
      increment: "commitIncrement",
      decrement: "commitDecrement",
    }),

    ...mapActions({
      //需要在actions逻辑的函数写到mapActions中即可
      incrementOdd: "incrementOdd",
      incrementWait: "incrementWait",
    }),
  },
};
</script>

<style>
button {
  margin-left: 5px;
}
</style>

```


#### 114-多组件共享数据

多组件共享数据案例实现代码：


```
//Count.vue组件中代码

<template>
  <div>
    <h2>当前求和为：{{ he }}</h2>
    <h3>当前求和放大十倍之后为：{{ bigSum }}</h3>
    <h3>我在{{ xuexiao }}学习{{ xueke }}</h3>

    <h2>Person组件的总人数为：{{personList.length}}</h2>
    <select v-model.number="curr">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
      <option value="5">5</option>
      <option value="6">6</option>
      <option value="7">7</option>
      <option value="8">8</option>
      <option value="8">9</option>
      <option value="10">10</option>
    </select>

    <button @click="increment(curr)">+</button>
    <button @click="decrement(curr)">-</button>
    <button @click="incrementOdd(curr)">当前求和为奇数时再加</button>
    <button @click="incrementWait(curr)">等一等再加</button>
  </div>
</template>

<script>
//引入mapState
import { mapGetters, mapState, mapMutations, mapActions} from "vuex";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Count",
  data() {
    return {
      curr: 1,
    };
  },
  //计算属性
  computed: {
    ...mapState({ he: "sum", xuexiao: "school", xueke: "subject", personList:'personList'}),
    ...mapGetters(["bigSum"]),
  },

  methods: {
    //使用mapMutations实现
    ...mapMutations({
      //不需要执行actions逻辑的函数可以直接使用mapMutations进行操作
      increment: "commitIncrement",
      decrement: "commitDecrement",
    }),

    ...mapActions({
      //需要在actions逻辑的函数写到mapActions中即可
      incrementOdd: "incrementOdd",
      incrementWait: "incrementWait",
    })
  },
};
</script>

<style>
button {
  margin-left: 5px;
}
</style>




//Person.vue组件中代码
<template>
  <div>
    <h2>Count组件中的求和为：{{sum}}</h2>
    <h3>人员列表</h3>
    <input type="text" placeholder="请输入名字：" v-model="inputName">
    <button @click="addUser">添加该人员</button>
    <ul v-for="p in personList" :key="p.id">
      <li>序号：{{p.id}}，姓名：{{p.name}}</li>
    </ul>
  </div>
</template>

<script>
//引入mapState
import {mapState} from 'vuex'
import {nanoid} from 'nanoid'
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Person",
    data(){
      return {
        inputName:""
      }
    },
    computed:{
      ...mapState({personList:"personList",sum:"sum"})
    },
    methods:{
      addUser(){
        const newPerson = {
          id:nanoid(),
          name:this.inputName
        }

        //console.log('newPerson : ',newPerson)

        //调用store中的函数
        this.$store.commit("commitAddPerson", newPerson)
      }
    }
}
</script>

<style scoped>
</style>



//store/index.js中代码

import { createStore } from 'vuex'
//

//Vue3中创建store对象
const store = createStore({
    state: {
        sum: 0,
        school: "xjtu",
        subject: "ai",
        personList: [//设置的保存person的列表
            {id:"001",name:"wyy"},
        ]
    },
    actions: {

        // increment: function (context, value) {
        //     console.log("increment actions 调用了...")
        //     //调用store.commit
        //     context.commit('commitIncrement', value)
        // },
        // decrement: function (context, value) {
        //     console.log("decrement actions 调用了...")
        //     context.commit("commitDecrement", value)
        // },

        incrementOdd(context, value) {
            console.log(context)
            console.log("incrementOdd actionos 调用了...")
            if (context.state.sum % 2) {
                context.commit('commitIncrementOdd', value)
            }
        },
        incrementWait(context, value) {
            console.log('incrementWait actions 调用了...')
            //加入定时器逻辑
            setTimeout(() => {
                context.commit('commitIncrementWait', value)
            },500)
        }
    },
    mutations: {
        commitIncrement: function (state, value) {
            console.log("commitIncrement mutations调用了...")
            state.sum += value
        },
        commitDecrement: function (state, value) {
            console.log("commitDecrement mutations 调用了...")
            state.sum -= value
        },
        commitIncrementOdd(state, value) {
            console.log("commitIncrementOdd mutations 调用了...")
            state.sum += value
        },
        commitIncrementWait(state, value) {
            console.log('commitIncrementWait mutations 调用了...')
            state.sum += value
        },
        //增加人员的函数
        commitAddPerson(state,value) {
            console.log("commitAddPerson mutations 调用了...")
            state.personList.unshift(value)
        }

    },
    getters: {
        //用于处理sum放大十倍之后的逻辑
        bigSum(state) {
            return state.sum * 10
        }
    }
})

//向外暴露store对象
export default store


```



#### 115-vuex模块化编码+namespace_1
实现案例代码：


```
//store/index.js文件中代码
import { createStore } from 'vuex'


//模块化编程
const countOptions = {
    namespaced: true,//写上namespace之后，在vue组件中调用mapState属性时，就可以使用下方store中的name:value中的name值进行定位。
    state: {
        sum: 0,
        school: "xjtu",
        subject: "ai",
    },

    actions: {

        // increment: function (context, value) {
        //     console.log("increment actions 调用了...")
        //     //调用store.commit
        //     context.commit('commitIncrement', value)
        // },
        // decrement: function (context, value) {
        //     console.log("decrement actions 调用了...")
        //     context.commit("commitDecrement", value)
        // },

        incrementOdd(context, value) {
            console.log(context)
            console.log("incrementOdd actionos 调用了...")
            if (context.state.sum % 2) {
                context.commit('commitIncrementOdd', value)
            }
        },
        incrementWait(context, value) {
            console.log('incrementWait actions 调用了...')
            //加入定时器逻辑
            setTimeout(() => {
                context.commit('commitIncrementWait', value)
            }, 500)
        }
    },

    mutations: {
        commitIncrement: function (state, value) {
            console.log("commitIncrement mutations调用了...")
            state.sum += value
        },
        commitDecrement: function (state, value) {
            console.log("commitDecrement mutations 调用了...")
            state.sum -= value
        },
        commitIncrementOdd(state, value) {
            console.log("commitIncrementOdd mutations 调用了...")
            state.sum += value
        },
        commitIncrementWait(state, value) {
            console.log('commitIncrementWait mutations 调用了...')
            state.sum += value
        },
    },

    getters: {
        //用于处理sum放大十倍之后的逻辑
        bigSum(state) {
            return state.sum * 10
        }

    }
}


const personOptions = {
    namespaced:true,
    state: {
        personList: [//设置的保存person的列表
            { id: "001", name: "wyy" },
        ]
    },

    actions: {

    },
    mutations: {
        //增加人员的函数
        commitAddPerson(state, value) {
            console.log("commitAddPerson mutations 调用了...")
            state.personList.unshift(value)
        }

    },
    getters: {

    },

}

//Vue3中创建store对象
const store = createStore({

    //模块化编程使用modules属性
    modules: {
        countAbout: countOptions,
        personAbout: personOptions
    }
})

//向外暴露store对象
export default store




//Count.vue组件中代码
<template>
  <div>
    <h2>当前求和为：{{ sum }}</h2>
    <h3>当前求和放大十倍之后为：{{ bigSum }}</h3>
    <h3>我在{{ school }}学习{{ subject }}</h3>

    <h2>Person组件的总人数为：{{ personList.length }}</h2>
    <select v-model.number="curr">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
      <option value="4">4</option>
      <option value="5">5</option>
      <option value="6">6</option>
      <option value="7">7</option>
      <option value="8">8</option>
      <option value="8">9</option>
      <option value="10">10</option>
    </select>

    <button @click="increment(curr)">+</button>
    <button @click="decrement(curr)">-</button>
    <button @click="incrementOdd(curr)">当前求和为奇数时再加</button>
    <button @click="incrementWait(curr)">等一等再加</button>
  </div>
</template>

<script>
//引入mapState
import { mapGetters, mapState, mapMutations, mapActions } from "vuex";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Count",
  data() {
    return {
      curr: 1,
    };
  },
  //计算属性
  computed: {
    ...mapState("countAbout", ["sum", "school", "subject"]), //直接引入a,b对应的module模块
    ...mapState("personAbout", ["personList"]), //直接引入a,b对应的module模块
    //getters方法调用
    ...mapGetters("countAbout", ["bigSum"]),
  },

  methods: {
    //使用mapMutations实现
    ...mapMutations("countAbout", {
      //不需要执行actions逻辑的函数可以直接使用mapMutations进行操作
      increment: "commitIncrement",
      decrement: "commitDecrement",
    }),

    ...mapActions("countAbout", {
      //需要在actions逻辑的函数写到mapActions中即可
      incrementOdd: "incrementOdd",
      incrementWait: "incrementWait",
    }),
  },
};
</script>

<style>
button {
  margin-left: 5px;
}
</style>


```

#### 116-Vuex模块化编程+namespace_2

案例实现代码：

```
//count.js代码
//模块化编程
const countOptions = {
    namespaced: true,//写上namespace之后，在vue组件中调用mapState属性时，就可以使用下方store中的name:value中的name值进行定位。
    state: {
        sum: 0,
        school: "xjtu",
        subject: "ai",
    },

    actions: {

        // increment: function (context, value) {
        //     console.log("increment actions 调用了...")
        //     //调用store.commit
        //     context.commit('commitIncrement', value)
        // },
        // decrement: function (context, value) {
        //     console.log("decrement actions 调用了...")
        //     context.commit("commitDecrement", value)
        // },

        incrementOdd(context, value) {
            console.log(context)
            console.log("incrementOdd actionos 调用了...")
            if (context.state.sum % 2) {
                context.commit('commitIncrementOdd', value)
            }
        },
        incrementWait(context, value) {
            console.log('incrementWait actions 调用了...')
            //加入定时器逻辑
            setTimeout(() => {
                context.commit('commitIncrementWait', value)
            }, 500)
        }
    },

    mutations: {
        commitIncrement: function (state, value) {
            console.log("commitIncrement mutations调用了...")
            state.sum += value
        },
        commitDecrement: function (state, value) {
            console.log("commitDecrement mutations 调用了...")
            state.sum -= value
        },
        commitIncrementOdd(state, value) {
            console.log("commitIncrementOdd mutations 调用了...")
            state.sum += value
        },
        commitIncrementWait(state, value) {
            console.log('commitIncrementWait mutations 调用了...')
            state.sum += value
        },
    },

    getters: {
        //用于处理sum放大十倍之后的逻辑
        bigSum(state) {
            return state.sum * 10
        }

    }
}

export default countOptions




//person.js文件代码

//引入axios
import axios from 'axios'
import { nanoid } from 'nanoid'

//配置personOptions选项
const personOptions = {
    namespaced: true,
    state: {
        personList: [//设置的保存person的列表
            { id: "001", name: "wyy" },
        ]
    },

    actions: {
        //设置只能增加姓王的人
        addPersonWang(context, value) {
            if (value.name.indexOf('王') === 0) {
                //提交本次请求
                context.commit('commitAddPerson', value)
                console.log("增加姓王的人成功...")
            } else {
                console.log('该用户不姓王，增加失败...')
            }
        },
        
        //从服务器中获得一个人的姓名
        getPersonFromServer(context) {
            axios.get('https://api.uixsj.cn/hitokoto/get?type=social').then(
                response => {
                    //创建person对象
                    const person = {
                        id: nanoid(),
                        name:response.data
                    }
                    context.commit('commitAddPerson', person)
                },
                
                error => {
                    console.log('请求失败，错误信息为：', error.message)
                }
            )
        }

    },

    mutations: {
        //增加人员的函数
        commitAddPerson(state, value) {
            console.log("commitAddPerson mutations 调用了...")
            state.personList.unshift(value)
        }

    },
    getters: {
        //获得列表中第一个person的姓名
        getFirstPersonName(state) {
            //返回值name
            return state.personList[0].name
        }

    },

}

export default personOptions


//src/store/index.js代码
import { createStore } from 'vuex'
import countOptions from './count'
import personOptions from './person'


//Vue3中创建store对象
const store = createStore({

    //模块化编程使用modules属性
    modules: {
        countAbout: countOptions,
        personAbout: personOptions
    }
})

//向外暴露store对象
export default store


/.vue组件中的代码保持不变即可



```



#### 117-Vue中的路由简介

路由的简介：
1.路由就是一组key:value的对应关系；
2.多个路由需要经过路由器的管理；


重点：提倡现在的web应用都做为SPA应用，即单页面应用。


Vue router会实时检测应用中路径的变化，此时key就是路径，value就是不同的组件。


vue-router是一个插件，可以监听应用路径的改变。


SPA单页面应用的数据获取需要请求ajax实现。


路由的分类：
1.前端路由：Vue-router控制前端页面的切换，在不同组件之间的切换；
2.后端路由：value是一个function，用于处理客户端提交的请求，过程：服务器接收到一个请求，找到请求中的function之后对请求进行处理，同时返回响应数据；




#### 118-路由的基本使用


路由的基本使用案例代码：

```
//main.js中代码
//引入Vue
import { createApp } from 'vue'

//引入store对象
// import store from './store/index'

//引入App组件，它是所有组件的父组件
import App from './App.vue'

//引入vueRouter插件
// import vueRouter from 'vue-router'

//引入配置好的路由器
import router from './router/index.js'

//创建Vue实例对象
const app = createApp(App)

//引入Vuex插件
// import Vuex from 'vuex'

//在挂载之前使用Vuex插件
// app.use(Vuex)

//使用store对象
// app.use(store)

//使用路由器
app.use(router)

//使用vue-router插件
// app.use(vueRouter)


//完成最后的挂载
app.mount('#app')



//router/index.js中代码
//引入vue-router插件
import { createRouter, createWebHashHistory } from 'vue-router'   // 使用插件可以不加这个

//引入组件
import About from '../components/About.vue'
import Home from '../components/Home.vue'

//配置路径：key:value的对应关系
const routes = [
    { path: '/About', name: "About", component: About },
    { path: '/Home', name: "Home", component: Home },
]

//创建路由器对象
const router = createRouter({
    history: createWebHashHistory('/'),  // 编写后台就这么写
    routes,
})


export default router


/App.vue组件中代码
<template>
  <div>
    <div class="row">
      <div class="col-xs-offset-2 col-xs-8">
        <div class="page-header"><h2>Vue Router Demo</h2></div>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
          <router-link class="list-group-item" active-class="active" to="/About">About</router-link>
          <router-link class="list-group-item" active-class="active" to="/Home">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
    <script>
    //引入组件
    // import Home from './components/Home.vue'
    // import About from './components/About.vue'

export default {
  name: "App",
  components:{

  }
};
</script>


<style scoped>
</style>



//About.vue组件中代码
<template>
  <h2>我是About的内容</h2>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "About",
};
</script>

<style>
</style>



//Home.vue组件中代码
<template>
  <h2>我是Home的内容</h2>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Home",
};
</script>

<style>
</style>

```



#### 119-路由的几个基本注意点

组件分为：
1.路由组件；
2.一般组件；


一般组件即表示无需程序员手工写入的组件，直接在App.vue组件中引入并写在html标签体中的；路由组件为vue-router渲染后放入到App.vue中的组件。


注意：
pages文件夹中主要存放路由组件；
components文件夹中主要存放一般组件；


重点：不使用的路由组件实际上是被Vue销毁了。



#### 120-嵌套路由（也叫做多级路由）

多级路由实现代码：

```

//router/index.js文件中的配置
//引入vue-router插件
import { createRouter, createWebHashHistory } from 'vue-router'   // 使用插件可以不加这个

//引入组件
import About from '../pages/About.vue'
import Home from '../pages/Home.vue'
import Message from '../pages/Message.vue'
import News from '../pages/News.vue'


//配置路径：key:value的对应关系
const routes = [
    {
        path: '/about', name: "about", component: About
    },
    {
        path: '/home',
        name: "home", component: Home,
        children: [
            { path: "message", name: "message", component: Message },
            { path: "news", name: "news", component: News }
        ]
    },
]

//创建路由器对象
const router = createRouter({
    history: createWebHashHistory('/'),  // 编写后台就这么写
    routes,
})


export default router



//Home.vue组件中配置
<template>
  <div>
    <h2>Home组件内容</h2>
    <div>
      <ul class="nav nav-tabs">
        <li>
          <router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>
        </li>
        <li>
          <router-link class="list-group-item" active-class="active" to="/home/message">Message</router-link>
        </li>
      </ul>
      <!--引入路由组件-->
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Home",
};
</script>

<style>
</style>



```

//截止到2022.9.2上午10：13止





