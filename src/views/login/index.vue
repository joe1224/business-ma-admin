<template>
  <div id="particles">
    <el-card class="login-form-layout">
      <el-form autoComplete="on"
               :model="loginForm"
               :rules="loginRules"
               ref="loginForm"
               label-position="left">
        <div style="text-align: center;margin-bottom: 0.8em;">
          <svg-icon icon-class="login-icon" style="width: 56px;height: 56px;color: #409EFF;vertical-align: middle;"></svg-icon>
          <span class="login-title">后台系统</span>
        </div>

        <el-form-item prop="username">
          <el-input name="username"
                    type="text"
                    v-model="loginForm.username"
                    autoComplete="on"
                    placeholder="请输入用户名">
          <span slot="prefix">
            <svg-icon icon-class="user" class="color-main"></svg-icon>
          </span>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input name="password"
                    :type="pwdType"
                    @keyup.enter.native="handleLogin"
                    v-model="loginForm.password"
                    autoComplete="on"
                    placeholder="请输入密码">
            <span slot="prefix">
              <svg-icon icon-class="password" class="color-main"></svg-icon>
            </span>
            <span slot="suffix" @click="showPwd">
                <svg-icon icon-class="eye" class="color-main"></svg-icon>
            </span>
          </el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-input name="code"
                    type="text"
                    autoComplete="on"
                    placeholder="请输入验证码"
          style="width: 60%;">
          <span slot="prefix">
            <svg-icon icon-class="code" class="color-main"></svg-icon>
          </span>
          </el-input>
          <img class="login-code" src="../../assets/images/captcha.jpg" alt="点击图片刷新">
        </el-form-item>
        <el-form-item>
          <el-button style="width: 100%" type="primary" :loading="loading" @click.native.prevent="handleLogin">
            登录
          </el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
  import {isvalidUsername} from '@/utils/validate';
//  import login_center_bg from '@/assets/images/login_center_bg.png'
  import particles from 'particles.js'
  import particlesConfig from "@/assets/particles.json";

  export default {
    name: 'login',
    mounted(){
      particlesJS("particles", particlesConfig);
//      document.body.style.overflow = "hidden";
    },
    data() {
      const validateUsername = (rule, value, callback) => {
        if (!isvalidUsername(value)) {
          callback(new Error('请输入正确的用户名'))
        } else {
          callback()
        }
      };
      const validatePass = (rule, value, callback) => {
        if (value.length < 3) {
          callback(new Error('密码不能小于3位'))
        } else {
          callback()
        }
      };
      return {
        loginForm: {
          username: 'admin',
          password: '123456'
        },
        loginRules: {
          username: [{required: true, trigger: 'blur', validator: validateUsername}],
          password: [{required: true, trigger: 'blur', validator: validatePass}]
        },
        loading: false,
        pwdType: 'password',
//        login_center_bg
      }
    },
    methods: {
      showPwd() {
        if (this.pwdType === 'password') {
          this.pwdType = ''
        } else {
          this.pwdType = 'password'
        }
      },
      handleLogin() {
        this.$refs.loginForm.validate(valid => {
          if (valid) {
            this.loading = true;
            this.$store.dispatch('Login', this.loginForm).then(() => {
              this.loading = false;
              this.$router.push({path: '/'})
            }).catch(() => {
              this.loading = false
            })
          } else {
            console.log('参数验证不合法！');
            return false
          }
        })
      }
    }
  }
</script>

<style scoped>
  .login-form-layout {
    position: absolute;
    left: 0;
    right: 0;
    width: 360px;
    margin: 230px auto;
    /*border-top: 10px solid #409EFF;*/
  }

  .login-title {
    text-align: center;
    font-weight: bold;
  }

  .login-code {
    width: 35%;vertical-align: middle;height: 40px;margin-left: 0.8em;
  }

  .login-center-layout {
    background: #409EFF;
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: 100%;
    margin-top: 200px;
  }

  #particles{
    position: absolute;
    width: 100%;
    height: 100%;
    background: #28AFE3 url(../../assets/images/bg1.png) no-repeat;
    background-size: cover;
    background-position: 50% 50%;
  }
</style>
