<template>
  <div class="venue-layout">
    <!-- Tab 切换栏 -->
    <el-tabs v-model="activeTab" type="card" @tab-click="handleTabClick">
      <el-tab-pane label="场地编排表" name="schedule"></el-tab-pane>
      <el-tab-pane label="场地管理" name="management"></el-tab-pane>
    </el-tabs>

    <!-- 场地编排表内容（示例） -->
    <div v-if="activeTab === 'schedule'" class="tab-content">
      <VenueManagement />
    </div>

    <!-- 场地管理内容（复用之前的组件） -->
    <div v-else class="tab-content">
      <!-- 引入之前写的场地管理组件 -->
      <SiteManagement />
    </div>
  </div>
</template>

<script>
// 引入场地管理组件（路径根据实际项目调整）
import VenueManagement from './VenueManagement.vue'
import SiteManagement from './SiteManagement.vue'

export default {
  name: 'VenueTabs',
  components: { VenueManagement,SiteManagement },
  data() {
    return {
      activeTab: 'schedule' // 默认选中“场地编排表”
    }
  },
  methods: {
    handleTabClick(tab) {
      console.log('切换到：', tab.name)
      this.activeTab=tab.name
      // 可在此添加 Tab 切换时的额外逻辑（如请求数据）
    }
  }
}
</script>

<style scoped>
.venue-layout {
  padding: 20px;
}
.el-tabs--card {
  margin-bottom: 20px;
}
.tab-content {
  background: #fff;
  padding: 20px;
  border: 1px solid #ebeef5;
  border-radius: 4px;
}
</style>
