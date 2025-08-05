<template>
  <div class="login" >
    
    <!-- 左侧面板 -->
    <div class="split-panel left-panel"  :class="{ hidden: is60Percent }" >
      <div class="left-top" :class="{ hidden: is60Percent }">
        <img  alt="" class="left-top-img" src="@/assets/logo/logo.png">
        <span class="left-top-font"   >   <!--:class="{ fontcolor: is60Percent }"-->
          安徽24365大学生就业服务云平台
        </span>
      </div>
      <div class="panel-content">
        <img key="1" alt="" class="w-350px" src="@/assets/svgs/login-box-bg.svg" />
        <div key="2" class="text-white text-3xl">欢迎使用本系统</div>
        <div key="3" class="address text-white">ahbys.com</div>
      </div>
    </div>


 <!-- 右侧面板 -->
    <div class="split-panel right-panel" :class="{ expanded: is60Percent }">
      <div class="right-top" :class="{ isshow: is60Percent }">
        <img  alt="" class="left-top-img" src="@/assets/logo/logo.png">
        <span class="right-top-font" >
          安徽24365大学生就业服务云平台
        </span>
      </div>
       <Ustc  v-if="showUstc"  @ustc-event="handleUstcEvent" :show-company="showCompany" />
       <Company v-else   @company-event="handleCompanyEvent"/>
    </div>

    <!--  底部  -->
    <!-- <div class="el-login-footer">
      <span>Copyright © 2018-2022 ruoyi.vip All Rights Reserved.</span>
    </div> -->
  </div>
</template>

<script>
import { getCodeImg } from "@/api/login";
import {Company ,Ustc }  from './components/login';
// import Company  from './components/login/Company';
// import Ustc  from './components/login/Ustc';
import Cookies from "js-cookie";
import { encrypt, decrypt } from '@/utils/jsencrypt'
export default {
  name: "Login",
  components:{
    Company,
    Ustc
  },
  data() {
    return {
      codeUrl: "",
       // 屏幕宽度
      screenWidth: window.screen.width,
      // 页面宽度
      pageWidth: window.innerWidth,
      // 页面/屏幕宽度比例
      ratio: 0,
      // 60%目标宽度
      targetWidth: 0,
      // 是否达到60%比例
      is60Percent: false,
      loading: false,
      // 验证码开关
      captchaEnabled: true,
      //单位注册开关
      companyRegister: true,
      // 注册开关
      register: false ,
      redirect: undefined,
      //登录和单位注册来回切换
      showUstc: true,
      //负责更改登录中ustc 和company 的值
      showCompany:false
    };
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true
    }
  },
  created() {
  },
  methods: {

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
    // 处理子组件传递过来的数据
    handleCompanyEvent(data) {
      this.showUstc = data
      this.showCompany=true
      
    },
    handleUstcEvent(data){
      console.log(data)
      this.showUstc = data;
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
.login {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-image: url("../assets/images/login-background.jpg");
  background-size: cover;
}

.split-panel {
  width: 50%;
  height: 100%;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}



.left-panel {
  background-image: url("../assets/images/login-left.png");
  border-right: 1px solid #e2e8f0;
  flex-direction: column;
}


.right-panel {
  background-color: #f8fafc;
  padding: 20px;
  color: #f8fafc;
  overflow-y: auto;
  flex-direction: column;
  /* max-height: 800px;  */
}

.left-top-img{
  margin-right: 10px;
  height: 43px;
  width: 43px;
}

.left-top-font{
  font-weight: 600;
  font-size: 18px;
  --un-text-opacity: 1;
  color: rgb(255 255 255 / var(--un-text-opacity));
  text-align: center;
}


.right-top-font{
  font-weight: 600;
  font-size: 18px;
  --un-text-opacity: 1;
  color: #303133;
  text-align: center;
}





.left-top{
  position: fixed; /* 固定在窗口左上角 */
  top: 0;
  left: 0;
  z-index: 100; /* 确保在其他内容上方 */
  width: 100%;
  padding: 0.8rem 1.7rem;
  /*border-bottom: 1px solid #bae6fd;  底部边框分隔 */
  box-sizing: border-box; /* 确保padding不影响宽度 */
  margin-top: 13px;
  display: flex;
  align-items: center; /* 垂直方向居中对齐 */
}


.right-top{
  display: none;
  
}

.right-top.isshow{
  display: flex;
  padding: 0.8rem 1.7rem;
  position: static;
  margin-top: 0px;
  box-sizing: border-box;
  margin-top: -71px;
  margin-left: -43px;
  z-index: 100; /* 确保在其他内容上方 */
  width: 100%;
  align-items: center; /* 垂直方向居中对齐 */
  box-sizing: border-box; /* 确保padding不影响宽度 */
}

.w-350px{
  width: 330px;
}
.text-white{
  --un-text-opacity: 1;
  color: rgb(255 255 255 / var(--un-text-opacity));
}

.text-3xl{
  font-size: 1.7rem;
  line-height: 2.25rem;
}

.address {
    font-size: 14px;
    font-weight: 400;
    margin-top: 1.25rem;
}


.panel-content {
  flex: 1; /* 占据剩余全部空间 */
  padding: 2rem;
  max-width: 500px;
  width: 100%;
  margin: 0 auto; /* 水平居中 */
  display: flex;
  flex-direction: column;
  justify-content: center; /* 垂直居中 */
  box-sizing: border-box;
}




/* 当左侧面板隐藏时，右侧面板占满容器宽度 */
.left-panel.hidden {
  display: none;
}

.right-panel.expanded {
  width: 100%;
}


/* 基础响应式调整 */
@media (max-width: 768px) {
  .container {
    width: 95%;
  }
}


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
/* .el-login-footer {
  height: 40px;
  line-height: 40px;
  position: fixed;
  bottom: 0;
  width: 100%;
  text-align: center;
  color: #fff;
  font-family: Arial;
  font-size: 12px;
  letter-spacing: 1px;
} */
.login-code-img {
  height: 38px;
}
</style>
