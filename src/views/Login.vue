<template>
  <el-card class="el-card" header="登录">
    <div slot="header">
      <el-row justify="center">
        <el-button type="text">登录</el-button>
        <el-divider direction="vertical" />
        <el-button type="text">注册</el-button>
      </el-row>
    </div>
    <el-form ref="loginForm" :model="loginForm" :rules="rules">
      <el-form-item prop="username">
        <el-input v-model="loginForm.username" prefix-icon="el-icon-user" placeholder="用户名或邮箱"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" placeholder="请输入密码" show-password></el-input>
      </el-form-item>
      <el-button type="primary" round @click="submitForm('loginForm')" class="login">登录</el-button>
    </el-form>
  </el-card>
</template>
<script>
  import Qs from 'qs';
  export default {
    data() {
      return {
        loginForm: {
          username: '',
          password: '',
        },
        rules:{
          username:[
            { required: true, message: '请输入用户名', trigger: 'blur' },
          ],
          password: [
            {required:true, message:'请输入密码',trigger: 'blur'}
          ]
        }
      };
    },
    methods: {
      login() {
        this.$axios.post("/login", Qs.stringify(this.$data.loginForm)).then(data => {
          this.$store.state.token = data.data;
          this.$message.info(JSON.stringify(data.data));
          // 跳转到首页
          this.$router.push('/');
        });
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (!valid) {
            return false;
          }
          this.login();
        });
      }
  }
  }
</script>
<style scoped>
  .el-card {
    text-align: center;
    width: 350px;
    margin: 0 auto;
  }

  .login {
    width: 85%;
  }

  .text-default {
    color: #000;
  }
</style>
