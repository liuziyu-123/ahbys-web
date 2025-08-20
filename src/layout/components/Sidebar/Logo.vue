<template>
  <div class="sidebar-logo-container" :class="{'collapse':collapse}"
       :style="{ backgroundColor: sideTheme === 'theme-dark' ? variables.menuBackground : variables.menuLightBackground }"
  >
    <transition name="sidebarLogoFade">
      <router-link v-if="collapse" key="collapse" class="sidebar-logo-link" to="/">
        <div class="logo-content">
          <img v-if="logo" :src="logo" class="sidebar-logo"/>
          <h1 v-else class="sidebar-title"
              :style="{ color: sideTheme === 'theme-dark' ? variables.logoTitleColor : variables.logoLightTitleColor }"
          >{{ title }} </h1>
        </div>
      </router-link>
      <router-link v-else key="expand" class="sidebar-logo-link" to="/">
        <div class="logo-content">
          <img v-if="logo" :src="logo" class="sidebar-logo"/>
          <h1 class="sidebar-title"
              :style="{ color: sideTheme === 'theme-dark' ? variables.logoTitleColor : variables.logoLightTitleColor }"
          >{{ title }} </h1>
        </div>
      </router-link>
    </transition>
  </div>
</template>

<script>
import logoImg from '@/assets/logo/logo.png'
import variables from '@/assets/styles/variables.scss'

export default {
  name: 'SidebarLogo',
  props: {
    collapse: {
      type: Boolean,
      required: true
    }
  },
  computed: {
    variables() {
      return variables
    },
    sideTheme() {
      return this.$store.state.settings.sideTheme
    }
  },
  data() {
    return {
      title: '安徽24365大学生就业服务平台',
      logo: logoImg
    }
  }
}
</script>

<style lang="scss" scoped>
.sidebarLogoFade-enter-active {
  transition: opacity 1.5s;
}

.sidebarLogoFade-enter,
.sidebarLogoFade-leave-to {
  opacity: 0;
}

.sidebar-logo-container {
  position: relative;
  width: 100%;
  height: 50px;
  line-height: 50px;
  background: #2b2f3a;
  //text-align: center;
  overflow: hidden;
  margin-top: 10px;

  & .sidebar-logo-link {
    display: flex;
    align-items: flex-start;
    height: 100%;
    width: 100%;
    padding: 0 16px; // 增加内边距让内容不贴边

    & .logo-content {
      display: flex;
      align-items: center; // 垂直居中对齐
      //white-space: nowrap; // 防止标题换行
      max-width: 100%; /* 限制最大宽度，防止溢出 */
    }

    & .sidebar-logo {
      width: 32px;
      height: 32px;
      //vertical-align: middle;
      margin-right: 12px;
      flex-shrink: 0; /* 防止图片被压缩 */
    }

    & .sidebar-title {
      //display: inline-block;
      margin: 0;
      color: #fff;
      font-weight: 600;
      line-height: 50px;
      font-size: 14px;
      font-family: Avenir, Helvetica Neue, Arial, Helvetica, sans-serif;
      line-height: 1.5; /* 调整行高，优化换行后的间距 */
      word-break: break-word; /* 允许单词内换行 */
      flex-shrink: 1; /* 标题可收缩 */
      //vertical-align: middle;
    }
  }

  &.collapse {
    .sidebar-logo {
      margin-right: 0px;
    }
    .sidebar-title {
      display: none; // 折叠时隐藏标题
    }
  }
}
</style>
