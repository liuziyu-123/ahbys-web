<template>
  <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form">
    <h3 class="title">登录</h3>
    <el-form-item prop="tenantName">
      <!-- <el-input
            v-model="loginForm.tenantName"
            type="text"
            auto-complete="off"
            disabled 
            placeholder="请输入租户名称"
          >
          <svg-icon slot="prefix" icon-class="house" class="el-input__icon input-icon" />
          </el-input> -->
      <el-select v-model="loginForm.tenantName" placeholder="请输入租户名称" style="width: 100%">
        <el-option v-for="item in tenantNames" :key="item.value" :label="item.label" :value="item.value">
        </el-option>
        <svg-icon slot="prefix" icon-class="house" class="el-input__icon input-icon" />
      </el-select>
    </el-form-item>
    <el-form-item prop="username">
      <el-input v-model="loginForm.username" type="text" auto-complete="off" placeholder="请输入账号">
        <svg-icon slot="prefix" icon-class="user" class="el-input__icon input-icon" />
      </el-input>
    </el-form-item>
    <el-form-item prop="password">
      <el-input v-model="loginForm.password" type="password" auto-complete="off" placeholder="请输入密码"
        @keyup.enter.native="handleLogin">
        <svg-icon slot="prefix" icon-class="password" class="el-input__icon input-icon" />
      </el-input>
    </el-form-item>
    <el-form-item prop="code" v-if="captchaEnabled">
      <el-input v-model="loginForm.code" auto-complete="off" placeholder="请输入验证码" style="width: 63%"
        @keyup.enter.native="handleLogin">
        <svg-icon slot="prefix" icon-class="validCode" class="el-input__icon input-icon" />
      </el-input>
      <div class="login-code">
        <img :src="codeUrl" @click="getCode" class="login-code-img" />
      </div>
    </el-form-item>
    <el-checkbox v-model="loginForm.rememberMe" style="margin:0px 0px 25px 0px;">记住我</el-checkbox>
    <el-button type="text" @click="goToForgetPwd" style="margin:0px 0px 0px 290px;">忘记密码?</el-button>
    <el-form-item style="width:100%;">
      <el-button :loading="loading" size="medium" type="primary" style="width:100%;"
        @click.native.prevent="handleLogin">
        <span v-if="!loading">登 录</span>
        <span v-else>登 录 中...</span>
      </el-button>
      <div style="float: right;" v-if="register">
        <router-link class="link-type" :to="'/register'">立即注册</router-link>
      </div>
    </el-form-item>
    <el-form-item v-if="loginForm.tenantName !== 'USTC'">
      <el-button plain @click="goToCompany" style="width: 100%;">
        单 位 注 册
      </el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  import { getCodeImg } from "@/api/login";
  import Cookies from "js-cookie";
  import { encrypt, decrypt } from '@/utils/jsencrypt'
  export default {
    name: "Login",
    // 声明接收父组件传递的 props
    props: {
      // 方式1：简单声明
      // parentMsg: String,

      // 方式2：详细声明（推荐）
      showCompany: {
        type: Boolean,
        required: false,
        default: false
      },
      // tenantName: {
      //   type: String,
      //   required: false,
      //   default: ''
      // }
    },
    data() {
      return {
        codeUrl: "",
        loginForm: {
          username: "admin",
          password: "admin123",
          rememberMe: false,
          tenantName: "USTC",
          code: "1111",
          uuid: ""
        },
        loginRules: {
          tenantName: [
            { required: true, trigger: "blur", message: "请输入您的租户名称" }
          ],
          username: [
            { required: true, trigger: "blur", message: "请输入您的账号" }
          ],
          password: [
            { required: true, trigger: "blur", message: "请输入您的密码" }
          ],
          code: [{ required: true, trigger: "change", message: "请输入验证码" }]
        },
        loading: false,
        // 验证码开关
        captchaEnabled: true,
        // 注册开关
        register: false,
        redirect: undefined,
        tenantNames: [{
          value: 'COMPANY',
          label: 'COMPANY'
        }, {
          value: 'USTC',
          label: 'USTC'
        }],
      };
    },
    watch: {
      $route: {
        handler: function (route) {
          this.redirect = route.query && route.query.redirect;
        },
        immediate: true
      },
      showCompany: {
        handler(newVal, oldValue) {
          console.log(newVal == true)
          if (newVal) {
            this.loginForm.tenantName = 'COMPANY';
          }

        },
        immediate: true
      },

     'loginForm.tenantName':{
      handler(newVal, oldValue) {
          console.log(newVal)
          this.$emit('ustc-tenantName', newVal);

        },
        immediate: true
      }

      // tenantName:{
      //   handler(newVal, oldValue) {
      //     console.log(newVal, oldValue,"77777777")
         
      //     this.loginForm.tenantName = newVal;
          

      //   },
      //   immediate: true
      // }

    },
    created() {
      this.getCode();
      this.getCookie();
    },
    methods: {
      getCode() {
        getCodeImg().then(res => {
          this.captchaEnabled = res.captchaEnabled === undefined ? true : res.captchaEnabled;
          if (this.captchaEnabled) {
            this.codeUrl = "data:image/gif;base64," + res.img;
            this.loginForm.uuid = res.uuid;
          }
        });
      },
      getCookie() {
        const fullUrl = window.location.href;
        console.log("完整网址：", fullUrl); // 输出：http://localhost:8080/login?redirect=/home
        const username = Cookies.get("username");
        const password = Cookies.get("password");
        const rememberMe = Cookies.get('rememberMe')
        const tenantName = Cookies.get('tenantName')
        this.loginForm = {
          tenantName: tenantName === undefined ? this.loginForm.tenantName : tenantName,
          username: username === undefined ? this.loginForm.username : username,
          password: password === undefined ? this.loginForm.password : decrypt(password),
          rememberMe: rememberMe === undefined ? false : Boolean(rememberMe)
        };
      },
      handleLogin() {
        this.$refs.loginForm.validate(valid => {
          if (valid) {
            this.loading = true;
            if (this.loginForm.rememberMe) {


              Cookies.set("username", this.loginForm.username, { expires: 30 });
              Cookies.set("password", encrypt(this.loginForm.password), { expires: 30 });
              Cookies.set('rememberMe', this.loginForm.rememberMe, { expires: 30 });
            } else {
              Cookies.remove("username");
              Cookies.remove("password");
              Cookies.remove('rememberMe');
            }
            this.$store.dispatch("Login", this.loginForm).then(() => {
              this.$router.push({ path: this.redirect || "/" }).catch(() => { });
            }).catch(() => {
              this.loading = false;
              if (this.captchaEnabled) {
                this.getCode();
              }
            });
          }
        });
      },

      // 更新宽度信息和状态
      updateWidthStatus() {
        // 获取最新宽度值
        this.screenWidth = window.screen.width;
        this.pageWidth = window.innerWidth;

        // 计算比例和目标宽度
        this.ratio = (this.pageWidth / this.screenWidth) * 100;
        this.targetWidth = this.screenWidth * 0.6;

        // 判断是否达到或超过60%
        this.is60Percent = this.ratio <= 60;
      },

      //跳转单位注册
      goToCompany() {
        // 使用 $emit 触发自定义事件，并传递数据
        this.$emit('ustc-company', false);
      },
      //跳转忘记密码
      goToForgetPwd() {
        // 使用 $emit 触发自定义事件，并传递数据
        this.$emit('ustc-forgetPwd', false, this.loginForm.tenantName);

      }

    },
    mounted() {
      // 初始化
      this.updateWidthStatus();

      // 监听窗口大小变化
      window.addEventListener('resize', this.updateWidthStatus);
    },
    beforeDestroy() {
      // 移除事件监听
      window.removeEventListener('resize', this.updateWidthStatus);
    }
  };
</script>

<style rel="stylesheet/scss" lang="scss">
  .title {
    margin: 0px auto 30px auto;
    text-align: center;
    color: #707070;
    font-weight: 700;
    font-size: 1.5rem;
    line-height: 2rem;
  }


  .login-form {
    border-radius: 6px;
    /* background: #ffffff; */
    width: 500px;
    padding: 25px 25px 5px 25px;

    .el-input {
      height: 38px;

      input {
        height: 38px;
      }
    }

    .input-icon {
      height: 39px;
      width: 14px;
      margin-left: 2px;
    }
  }

  .login-tip {
    font-size: 13px;
    text-align: center;
    color: #bfbfbf;
  }

  .login-code {
    width: 33%;
    height: 38px;
    float: right;

    img {
      cursor: pointer;
      vertical-align: middle;
    }
  }

  .login-code-img {
    height: 38px;
  }
</style>