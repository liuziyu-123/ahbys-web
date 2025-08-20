<template>
  <div class="dashboard-chart">
    <div class="chart-title">各院系就业落实情况(点击查看详细)</div>
    <div class="chart-container">
      <!-- 这里只是简单模拟柱状图结构，实际可用 ECharts 渲染，需安装 echarts 依赖 -->
      <div class="bar" v-for="(item, index) in chartData" :key="index" :style="{ height: item.percent + '%', backgroundColor: '#3498db' }">
        <div class="bar-label">{{ item.label }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DashboardChart',
  data() {
    return {
      chartData: [
        { label: '院系1', percent: 6.52 },
        { label: '院系2', percent: 0 },
        { label: '院系3', percent: 0.55 },
        { label: '院系4', percent: 8.0 },
        { label: '院系5', percent: 19.21 },
        { label: '院系6', percent: 7.4 },
        { label: '院系7', percent: 9.73 },
        { label: '院系8', percent: 6.37 },
        { label: '院系9', percent: 1.54 },
        { label: '院系10', percent: 4.92 },
        { label: '院系11', percent: 4.11 },
        { label: '院系12', percent: 2.3 },
        { label: '院系13', percent: 1.88 },
        { label: '院系14', percent: 6.49 },
        { label: '院系15', percent: 8.76 },
        { label: '院系16', percent: 1.63 },
        { label: '院系17', percent: 0 },
        { label: '院系18', percent: 5.31 },
        { label: '院系19', percent: 3.08 },
        { label: '院系20', percent: 0.86 },
        { label: '院系21', percent: 6.53 },
        { label: '院系22', percent: 3.38 },
        { label: '院系23', percent: 2.05 },
        { label: '院系24', percent: 2.84 },
        { label: '院系25', percent: 0.82 },
        { label: '院系26', percent: 7.29 },
        { label: '院系27', percent: 5.83 },
        { label: '院系28', percent: 0 },
        { label: '院系29', percent: 2.56 },
        { label: '院系30', percent: 0 },
        { label: '院系31', percent: 1.8 },
        { label: '院系32', percent: 5.66 },
        { label: '院系33', percent: 2.6 },
        { label: '院系34', percent: 0.99 }
      ]
    };
  }
};
</script>

<style scoped>
.dashboard-chart {
  padding: 20px;
}
.chart-title {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}
.chart-container {
  display: flex;
  align-items: flex-end;
  height: 200px;
  border-left: 2px solid #ccc;
  border-bottom: 2px solid #ccc;
  padding: 10px;
}
.bar {
  width: 20px;
  margin-right: 5px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.bar-label {
  margin-top: 5px;
  font-size: 12px;
  color: #666;
}
</style>
