<template>
  <div class="dashboard-stats">
    <div class="stats-group">
      <div class="stat-item">
        <div class="label">毕业生总数</div>
        <div class="number">8112</div>
      </div>
      <div class="stat-item">
        <div class="label">就业落实人数</div>
        <div class="number">7413</div>
      </div>
      <div class="stat-item">
        <div class="label">就业落实率(实时)</div>
        <div class="number">91.38%</div>
      </div>
      <div class="stat-item">
        <div class="label">网签数</div>
        <div class="number">3776</div>
      </div>
      <div class="stat-item">
        <div class="label">单位数</div>
        <div class="number">9892</div>
      </div>
      <div class="stat-item">
        <div class="label">职位数</div>
        <div class="number">18101</div>
      </div>
      <div class="stat-item">
        <div class="label">岗位数</div>
        <div class="number">244923</div>
      </div>
      <div class="stat-item">
        <div class="label">简历数</div>
        <div class="number">592</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DashboardStats'
};
</script>

<style scoped>
.dashboard-stats {
  padding: 20px;
}
.stats-group {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}
.stat-item {
  background: #fff;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: calc(25% - 20px);
  min-width: 150px;
}
.label {
  font-size: 14px;
  color: #666;
  margin-bottom: 5px;
}
.number {
  font-size: 16px;
  font-weight: bold;
}
</style>
