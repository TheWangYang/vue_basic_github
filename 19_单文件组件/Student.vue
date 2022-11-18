<template>
    <!--组件的结构-->
    <div>
        <h2>学生姓名：{{studentName}}</h2>
        <h2>学生年龄：{{ studentAge }}</h2>
        <button @click="showName">点我提示学生姓名</button>
    </div>
</template>



<script>
    
    //另外一种不写Vue.extend()的方法
    export default {
        name:"Student",//定义组件的名称
        data(){
            return {
                studentName:"wyy",
                studentAge:18
            }
        },
        
        methods:{
            //点击显示学生姓名
            showName(){
                alert(this.studentName)
            }
        }
    }

</script>






