<template>
  <div id="login">
    <el-form :model="login" :rules="rules" ref="loginForm">
      <div class="logo">
        <img src="../assets/logo.png" alt />
      </div>
      <el-form-item prop="username">
        <el-input v-model="login.username" prefix-icon="el-icon-user" autofocus></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input show-password v-model="login.password" prefix-icon="el-icon-key"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="loginIn()">登录</el-button>
        <el-button type="info" @click="clearForm">清除</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      login: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, message: '密码过短', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    loginIn() {
      this.$refs.loginForm.validate(bool => {
        if (!bool) {
          console.log('验证失败')
        } else {
          this.$http.post('login', this.login).then(response => {
            if (response.data.meta.status !== 200) {
              this.$message.error('账号或密码错误')
            } else {
              this.$message.success(
                '登录成功 欢迎您：' + response.data.data.username + '!'
              )
              // 保存token之，用于其他api接口调用凭证
              window.sessionStorage.setItem('tooken', response.data.data.token)
              // 跳转页面
              this.$router.push('/home')
            }
          })
        }
      })
    },
    clearForm() {
      this.$refs.loginForm.resetFields()
    }
  }
}
</script>
<style lang="less" scoped>
#login {
  position: relative;
  width: 100%;
  height: 100%;
  background: url(../assets/img/bg.jpg) no-repeat 50% 50%;
}

.el-form {
  width: 350px;
  height: 300px;
  position: absolute;
  top: 50%;
  right: 50%;
  transform: translate(50%, -50%);
  padding: 50px 20px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  .logo {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 0;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 0;
    background: white;
    border-radius: 50%;
    overflow: hidden;
    img {
      width: 100%;
    }
  }
}
</style>
