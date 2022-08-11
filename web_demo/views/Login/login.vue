<template>
    <el-form
        :model="form"
        status-icon
        :rules="rules"
        ref="form"
        label-width="100px"
        class="login-container"
    >

    </el-form>
</template>

<script>
export default {
    name:"login",
    data(){
        return{
            form:{

            },
            rules:{
                username:[
                    { required:true,message:'请输入用户名',trigger:'blur' },
                    {
                        min:3,
                        message:'用户名长度不能小于三位',
                        trigger:'blur'
                    }
                ],
                password:[
                    { required:true,message:'请输入密码',trigger:'blur' }
                ]
            }
        }
    }    
}
</script>

<style lang="less" scoped>

</style>