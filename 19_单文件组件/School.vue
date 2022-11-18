<template>
    <!--组件的结构-->
    <div id="demo">
        
        <h2>学校名称：{{schoolName}}</h2>
        <h2>学校地址：{{ schoolAddress }}</h2>
        <button @click="showName">点我提示学校名称</button>
    </div>
</template>



<script>


    //另外一种不写Vue.extend()的方法
    export default {
        name:"School",//定义组件的名称
        data(){
            return {
                schoolName:"xjtu",
                schooleAddress:"西安"
            }
        },
        
        methods:{
            showName(){
                alert(this.schoolName)
            }
        }
    }

</script>



<style>
    /* 组件的样式 */

    .demo{
        background-color: orange;
    }



</style>






