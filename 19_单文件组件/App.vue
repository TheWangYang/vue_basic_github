<template>
<div>
    <!--组件的结构-->
    <School></School>
    <Student></Student>
</div>
    
</template>

<script>
//引入组件
import School from './School.vue'
import Student from './Student.vue'

    //另外一种不写Vue.extend()的方法
    export default {
        name:"App",//定义组件的名称
        components:{
            Student,
            School
        }
    }

</script>






