<template>
  <div class="login">
    <el-form ref="form" :model="form" :rules='rules' label-width="80px" label-position='top'>
      <el-form-item label="手机号" prop='phone'>
        <el-input v-model="form.phone"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop='password'>
        <el-input type="password" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" :loading='isLoginLoading' @click="onSubmit">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
// import request from '@/utils/request.js'
// import qs from 'qs'
// 引入封装的接口功能组件
import { login } from '@/services/user'
export default {
  name: 'Login',
  data () {
    return {
      form: {
        phone: '18201288771',
        password: '111111'
      },
      rules: {
        phone: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { pattern: /^1\d{10}$/, message: '请输入正确的手机号', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 18, message: '密码长度为 6 到 18 位', trigger: 'blur' }
        ]
      },
      // 用于保存加载状态
      isLoginLoading: false
    }
  },
  methods: {
    // 登录功能
    async onSubmit () {
      try {
        // 设置校验
        await this.$refs.form.validate()
        // 2.发送请求
        this.isLoginLoading = true
        // const { data } = await request({
        // method: 'POST',
        // url: '/front/user/login',
        // // urlencoded 格式：名=值&名=值
        // data: qs.stringify(this.form)
        // })
        const { data } = await login(this.form)
        this.isLoginLoading = false
        // 3.响应处理
        if (data.state === 1) {
          this.$router.push({
            name: 'home'
          })
          this.$message.success('登录成功')
        } else {
          this.$message.error('登录失败')
        }
      } catch (error) {
        // 设置检验失败后的功能（提示）
        console.log('没有通过检验')
      }
    }
  }
}

</script>
<style lang="scss" scoped>
.login {
  height: 100vh;
  display: flex;
  // 设置内部元素垂直水平居中
  justify-content: center;
  align-items: center;

  .el-form {
    background-color: #fff;
    padding: 20px;
    width: 300px;
    border-radius: 10px;

    .el-button {
      width: 100%;
    }
  }
}
</style>
