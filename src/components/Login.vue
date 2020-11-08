<template>
  <div class="login_container">
    <div class="login_box">

      <!--头像区域-->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt=""/>
      </div>

      <!--登录表单-->
      <el-form :model="loginForm"
               :rules="loginFormRules"
               label-width="0px"
               class="login_from"
               ref="loginFormRef"
      >
        <!--账号-->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-zhanghu">
          </el-input>
        </el-form-item>

        <!--密码-->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-password"
            type="password">
          </el-input>
        </el-form-item>

        <!--按钮-->
        <el-form-item class="login_buts">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>

      </el-form>

    </div>

  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      // 这里登录表单的数据绑定对象
      loginForm: {
        username: '',
        password: ''
      },
      // 这是表单的数据验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          {
            required: true,
            message: '请输入登录名称',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 10,
            message: '长度在 3 到 10 个字符',
            trigger: 'blur'
          }

        ],
        // 密码的验证规则
        password: [
          {
            required: true,
            message: '请输入登录密码',
            trigger: 'blur'
          },
          {
            min: 6,
            max: 15,
            message: '长度在 6 到 15 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    // 点击重置，重置表单并去除校验规则
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid
        if (!valid) return
        var { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  background-color: #fff;
  border-radius: 3px;
  height: 300px;
  width: 450px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.avatar_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;

  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}

.login_from {
  position: absolute;
  bottom: 0px;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.login_buts {
  display: flex;
  justify-content: flex-end;
}
</style>
