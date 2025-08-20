<template>
  <div class="dashboard-header">
    <div class="card-group">
      <div class="card" v-for="(item, index) in List" :key="index">
        <div class="title">{{ item.title }}</div>
        <div class="value" :style="{ color: item.color }">{{ item.value }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DashboardHeader',
  data() {
    return {
      List: [
        { title: '单位注册', value: 2, color: '#1abc9c' },
        { title: '招聘公告', value: 3, color: '#2ecc71' },
        { title: '招聘职位', value: 127, color: '#3498db' },
        { title: '宣讲会', value: 0, color: '#95a5a6' },
        { title: '双选会报名', value: 0, color: '#95a5a6' },
        { title: '就业信息审核', value: 1, color: '#f39c12' },
        { title: '全国系统上报错误', value: '42/54', color: '#27ae60' }
      ]
    };
  }
};
</script>

<style scoped>
.dashboard-header {
  padding: 20px;
}
.card-group {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}
.card {
  background: #fff;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: calc(25% - 20px);
  min-width: 150px;
}
.title {
  font-size: 14px;
  color: #666;
  margin-bottom: 8px;
}
.value {
  font-size: 18px;
  font-weight: bold;
}
</style>
