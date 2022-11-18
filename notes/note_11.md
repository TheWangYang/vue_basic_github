## 尚硅谷Vue系列教程学习笔记（11）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)




#### 0101-vue-resource

重点：vue-resource是Vue的插件库，其是对xhr的封装。


重点：使用vue-resource发送http请求的代码实例：

```

 //使用vue-resource插件发送http请求的代码
      this.$http.get(`xxxxx`).then(...);

```


重点：Vue3目前还无法完成对vue-resource插件的挂载。



#### 0102-默认插槽


分类展示案例代码：


```
//在App.vue组件中的代码
    <template>
  <div id="root">
    <div class="container">
      <Category title="美食">
        <img src="xxxx" />
      </Category>
      <Category title="游戏">
        <ul v-for="(game, index) in games" :key="index">
          <li>{{ game }}</li>
        </ul>
      </Category>
      <Category title="电影">
        <ul v-for="(film, index) in films" :key="index">
          <li>{{ film }}</li>
        </ul>
      </Category>
    </div>
  </div>
</template>
    <script>
//引入组件
import Category from "./components/Category.vue";

export default {
  name: "App",
  components: {
    Category,
  },
  data() {
    return {
      foods: ["火锅", "鱼豆腐", "甜不辣", "冰激淋"],
      films: ["test1", "test2", "test3", "test4"],
      games: ["game1", "game2", "game3", "game4"],
    };
  },
};
</script>


<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}

img{
  width: 100%;
}

</style>



//在Category.vue组件中代码如下
<template>
  <div class="categoty">
    <h3>{{title}}分类</h3>
    <!--表示将插值标签体内容放到这个位置-->
    <slot></slot>
  </div>
</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name:"Category",
    props:['listData','title']
}
</script>

<style>
.categoty{
    background-color: skyblue;
    width: 200px;
    height: 300px;
}

h3{
    text-align: center;
    background-color: orange;
}
</style>


```



#### 0103-具名插槽

具名插槽就是有名字的插槽，主要和第一种普通的插槽不同的地方为，

具名插槽案例代码：


```
//App.vue组件中代码
    <template>
  <div id="root">
    <div class="container">
      <Category title="美食">
        <template v-slot:center>
          <img src="xxxx" />
        </template>

        <template v-slot:footer>
          <a href="xxxxx">这个是test链接</a>
        </template>
      </Category>

      <Category title="游戏">
        <template v-slot:center>
          <ul v-for="(game, index) in games" :key="index">
            <li>{{ game }}</li>
          </ul>
        </template>

        <template v-slot:footer>
          <div class="gameFoot">
            <a href="xxxx">跳转地址1</a>
            <a href="xxxx">跳转地址2</a>
          </div>
        </template>
      </Category>
      <Category title="电影">
        <ul v-for="(film, index) in films" :key="index">
          <li>{{ film }}</li>
        </ul>
      </Category>
    </div>
  </div>
</template>
    <script>
//引入组件
import Category from "./components/Category.vue";

export default {
  name: "App",
  components: {
    Category,
  },
  data() {
    return {
      foods: ["火锅", "鱼豆腐", "甜不辣", "冰激淋"],
      films: ["test1", "test2", "test3", "test4"],
      games: ["game1", "game2", "game3", "game4"],
    };
  },
};
</script>


<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}

img {
  width: 100%;
}


.gameFoot{
  display: flex;
  justify-content: space-around;
}
</style>


//Catefgory.vue组件代码
<template>
  <div class="categoty">
    <h3>{{ title }}分类</h3>
    <!--表示将插值标签体内容放到这个位置-->
    <slot name="center">这个是中间位置的插槽</slot>
    <slot name="footer">这个是底部的插槽</slot>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Category",
  props: ["listData", "title"],
};
</script>

<style>
.categoty {
  background-color: skyblue;
  width: 200px;
  height: 300px;
}

h3 {
  text-align: center;
  background-color: orange;
}
</style>

```


#### 0104-作用域插槽

作用域插槽案例实现代码：


```
//App.vue组件中代码
    <template>
  <div id="root">
    <div class="container">
      <Category title="游戏">
        <template v-slot:center="data">
          <ul v-for="(game, index) in data.games" :key="index">
            <li>{{ game }}</li>
          </ul>
        </template>
      </Category>

      <Category title="游戏">
        <template v-slot:center="data">
          <ol v-for="(game, index) in data.games" :key="index">
            <li>{{ game }}</li>
          </ol>
        </template>
      </Category>

      <Category title="游戏">
        <template v-slot:center="data">
          <H4 v-for="(game, index) in data.games" :key="index">
            <li>{{ game }}</li>
          </H4>
        </template>
      </Category>
    </div>
  </div>
</template>
    <script>
//引入组件
import Category from "./components/Category.vue";

export default {
  name: "App",
  components: {
    Category,
  },
};
</script>


<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}

img {
  width: 100%;
}

.gameFoot {
  display: flex;
  justify-content: space-around;
}
</style>



//Category.vue组件中的代码

<template>
  <div class="categoty">
    <h3>{{ title }}分类</h3>
    <!--表示将插值标签体内容放到这个位置-->
    <slot name="center" :games="games">这个是测试作用域的插槽</slot>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Category",
  props: ["title"],
    data() {
    return {
      games: ["game1", "game2", "game3", "game4"],
    };
  }

};
</script>

<style>
.categoty {
  background-color: skyblue;
  width: 200px;
  height: 300px;
}

h3 {
  text-align: center;
  background-color: orange;
}
</style>


```


#### 0105-Vuex简介

重点：Vuex是专门在Vue中实现集中式状态管理的一个插件。

其中的状态就是数据，数据就可以理解为状态。

作用：对Vue中多个组件之间的共享状态进行集中式的管理，也是一种组件间的通信方式，且适用于任意组件间的通信。


什么时候使用Vuex：
1.多个组件依赖于同一个状态；
2.来自不同组件的行为需要变更同一状态；



#### 0106-求和案例-纯Vue版实现



案例代码：


```
<template>
  <div>
    <h2>当前求和为：{{ sum }}</h2>
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

    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数时再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Count",
  data() {
    return {
      curr: 1,
      sum: 0,
    };
  },
  methods: {
    increment() {
      this.sum += this.curr;
    },
    decrement() {
      this.sum -= this.curr;
    },
    incrementOdd() {
      if (this.sum % 2) {
        this.sum += this.curr;
      }
    },
    incrementWait() {
      setTimeout(() => {
        this.sum += this.curr;
      }, 500);
    },
  },
};
</script>

<style>
button {
  margin-left: 5px;
}
</style>

```



#### 0107-Vuex工作原理图

本小节重点：
分析Vuex的工作原理：

Vuex的三个重要组成部分：
1.actions：
2.mutations：
3.state：将数据交给Vuex管理实际上就是交给Vuex对象进行管理；


用户在组件中的动作，首先响应的人为Vuex中的actions对象。


Vuex解析动作并重新解析模板的流程：
首先，用户递交一个动作，调用dispatch()函数，其中传入参数('jia',2)，然后Vuex的actions对象中接收函数和其中的参数，同时，在actions中的jia键值对应的function()函数中调用commit('jia',2)函数，然后到达mutations对象中，在mutations对象中包含一个key:value键值对，即为：'jia':function()，然后将更改反映到Vuex中的state对象中（造成了数据的改变），而后VueComponents对象对数据进行重新解析即可。


actions对象的作用：actions接收前端组件的动作请求之后，有可能出现该请求中没有参数，而需要actions调用后端的api请求另外一台服务器或者其他接口得到，这就是actions存在的意义。


Vuex允许如果VueComponents知道自己的动作和参数的话，可以直接想mutations对象发送commit请求，而跳过actions对象处理的阶段。



actions/mutations/state三个对象需要store对象的领导。



#### 0108-初始化Vuex环境
步骤：
1.npm i vuex；
2.app.use(Vuex)；
3.store对象创建；
4.需要让所有组件VC都能看见store；


注意：vue2只能使用vuex3版本，vue3只能使用vuex4版本。


使用Vuex案例代码：

```
//在store文件夹下的index.js文件中写入如下代码
import Vuex from 'vuex'
import Vue from 'vue'


//Vue2中需要在创建store对象之前使用Vue.use(Vuex)
Vue.use(Vuex)


//准备actions对象
const actions = {}

//准备mutations对象
const mutations = {}

//准备state对象
const state = {}


//创建store对象，并向外暴露store
export default new Vuex.Store({
    actions: actions,
    mutations: mutations,
    state: state
})



//src目录下的main.js文件中写入如下代码
//引入Vue
import { createApp } from 'vue'

//引入App组件，它是所有组件的父组件
import App from './App.vue'

//引入store
import store from './store/index'

//创建Vue实例对象
const app = createApp(App)

//使用store对象
app.use(store)

//完成最后的挂载
app.mount('#app')

```

重点：需要在引入store对象之前引入并使用插件Vuex。



注意：在脚手架中引入import时，不管import出现的位置，js都会首先解析import引入的文件。



Vue3中引入store的案例代码：

```
//src/store/index.js文件中代码
import { createStore } from 'vuex'

//Vue3中创建store对象
const store = createStore({
    state: {},
    mutations: {},
    actions: {},
    modules: {

    }
})

//向外暴露store对象
export default store


//在main.js文件中引入代码
//引入Vue
import { createApp } from 'vue'

//引入store对象
import store from './store/index'

//引入App组件，它是所有组件的父组件
import App from './App.vue'

//创建Vue实例对象
const app = createApp(App)

//引入Vuex插件
import Vuex from 'vuex'

//在挂载之前使用Vuex插件
app.use(Vuex)

//使用store对象
app.use(store)


//完成最后的挂载
app.mount('#app')

```



#### 0109-求和案例Vuex版

总结：如果有业务逻辑，那么需要放入到actions中。


求和案例使用Vuex实现代码：

```
//Count.vue组件中代码
<template>
  <div>
    <h2>当前求和为：{{ $store.state.sum }}</h2>
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

    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数时再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Count",
  data() {
    return {
      curr: 1,
    };
  },
  methods: {
    increment() {
      //调用$store使用Vuex实现共享存储对象
      this.$store.dispatch("increment", this.curr);
    },
    decrement() {
      //调用$store使用Vuex实现共享存储对象
      this.$store.dispatch("decrement", this.curr);
    },
    incrementOdd() {
        //判断是否为奇数的逻辑在actions对象中
        this.$store.dispatch("incrementOdd", this.curr);
      
    },
    incrementWait() {
        // console.log("timeout inner this : ", this)
        //定时器逻辑在actions中
        this.$store.dispatch("incrementWait", this.curr);
      
    },
  },
  mounted() {
    console.log("Count.vue this : ", this);
  },
};
</script>

<style>
button {
  margin-left: 5px;
}
</style>


//store/index.js中代码
import { createStore } from 'vuex'

//Vue3中创建store对象
const store = createStore({
    state: {
        sum: 0
    },
    actions: {
        increment: function (context, value) {
            console.log("increment actions 调用了...")
            //调用store.commit
            context.commit('commitIncrement', value)
        },
        decrement: function (context, value) {
            console.log("decrement actions 调用了...")
            context.commit("commitDecrement", value)
        },
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
        }

    },
    modules: {

    }
})

//向外暴露store对象
export default store

```



#### 0110-Vuex开发者工具的使用

注意：Vue开发者工具和Vuex开发者工具合成了一个。

开发者工具只给mutations对话。


重点：在actions的每个函数中可以继续调用context.dispatch()函数继续调用其他处理动作逻辑的函数。




注意：如果直接在actions的函数中进行动作的实现（例如：数据的修改等），那么开发者工具就会失效，因为动作没有传递到mutations对象中，因此不建议这样做。


//截止到2022.9.1日下午15：57止








