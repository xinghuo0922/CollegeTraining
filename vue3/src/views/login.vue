<template>
  <div class="login_body">
      <div style="width: 400px; margin: 200px auto">
        <h1 style="text-align: center;color:brown;font-family: 微软雅黑">高校实训实习管理平台</h1>
        <div style="color: #cccccc;font-size: 50px;font-family: 隶书;text-align: center;padding: 30px 0">欢迎登录</div>
        <el-form :model="user" :rules="rules" ref="rulesFormRef" size="large">
          <el-form-item prop="username">
            <el-input v-model="user.username" clearable="clearable" placeholder="请输入用户名" :prefix-icon="User"/>
          </el-form-item>
          <el-form-item prop="password">
            <el-input v-model="user.password" show-password clearable="clearable" placeholder="请输入密码" :prefix-icon="Lock"/>
          </el-form-item>
          <el-form-item prop="power">
            <el-select id="power" v-model="user.power" placeholder="请选择您的职务" size="large">
              <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item>
          <el-button type="primary" style="width: 100%;" @click="login">登录</el-button>
        </el-form-item>
        </el-form>
      </div>
  </div>
</template>


<script setup>
import {ref, reactive, getCurrentInstance} from 'vue'
import {User,Lock} from '@element-plus/icons'
import {ElMessage, ElNotification} from "element-plus";
import request from "@/request";
const { proxy }=getCurrentInstance()

const user= reactive({
  username: '',
  password:'',
  power:'',
})
const changeUser=(name)=>{
  user.name=name
}

//表单校验
const rules= reactive({
  username: [
    { required: true, message: '请输入用户名', trigger: 'blur' },
    // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' },
  ],
  password: [
    {required: true, message: '请输入密码', trigger: 'blur'},
    // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' },
  ],
  // Bug： select选择器未弹出提示文字
  power: [
    {required:true,message:'请选择职务',trigger:'change'},
  //   // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' },
   ]
})

const options = [
  {
    value: '管理员',
    label: '管理员',
  },
  {
    value: '老师',
    label: '老师',
  },
  {
    value: '学生',
    label: '学生',
  },
]

//只有填写完整才让你登录
const login = ( ) => {
  proxy.$refs.rulesFormRef.validate((valid)=>{
      if(valid){
        //输入正确，则向后台发送信息,request中全局统一加上了 后端接口前缀 http://localhost:9090
        //res是后台返回的数据,
        request.post('/login').then(res =>{
          if(res.code === '200'){//请求成功
              ElNotification({
                type:"success",
                message:"登陆成功"
              })
          }else {
              ElNotification({
                type:'error',
                message: res.msg
              })
          }
        })
      }else{
        ElMessage({
          type:"error",
          message:'账号或密码不正确'
        })
      }
  })
}
</script>

<style>
  .login_body{
    width: 100%;
    height: 100%;
    min-width: 1000px;
    background-image: url("../assets/login.png");
    background-size: 100% 100%;
    background-position: center center;
    overflow: auto;
    background-repeat: no-repeat;
    position: fixed;
    line-height: 100%;
    padding-top: 150px;
  }
</style>