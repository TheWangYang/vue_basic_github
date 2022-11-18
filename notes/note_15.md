## 尚硅谷Vue系列教程学习笔记（15）
* 参考课程：《尚硅谷Vue2.0+Vue3.0全套教程丨vuejs从入门到精通》
* 参考链接：https://www.bilibili.com/video/BV1Zy4y1K7SH?vd_source=4f4d3466cdc2c5a2539504632c862ce7
* 笔记上传说明：上述课程中共有168节小课，对每10次课的笔记整合之后上传一次。
* 详细代码案例请移步本人gitee仓库： [vue_basic案例代码实现](https://gitee.com/SCKDKT/vue_basic)
* Vue-cli脚手架实现案例代码见仓库：[vue01](https://gitee.com/SCKDKT/vue01)



#### 141-初识setup

重点：常用的组合式API，Composition API。

注意：学习Vue3最重要的就是学习setup。

setup是所有组合式api表演的舞台。

setup是一个新的配置项，值为一个函数。


如果setup中返回一个对象，那么其中的属性和方法都可以直接在组件中使用。


重点：setup返回值为一个对象（常用）代码：
```
setup(){
    //如果返回值是一个对象，直接在组件中使用即可
    return {
        name:"test",
        age:22
    }
}


//直接在组件中使用即可
<div>我的名字为：{{name}}，年龄为：{{age}}</div>

```


重点：setup中使用渲染式的案例代码（不常用）代码：

```
 //使用渲染式案例代码
 setup(){
    return ()=>h('h2','test setup 渲染函数')
 }

```


重点注意：
1.在Vue2的配置中读取Vue3的setup中的属性和数据时是可以成功获得的，但是在Vue3中读取Vue2中的数据或函数是失败的。
2.在一个组件中如果使用了Vue2和Vue3共同的配置，那么在组件渲染时，主要是以Vue3为主。


#### Vue3中的ref函数-处理基本类型
重点：





