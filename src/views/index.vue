<template>
  <div class="dashboard">
    <DashboardHeader />
    <DashboardStats />
    <DashboardChart />
  </div>
</template>

<script>
import { DashboardHeader,DashboardStats,DashboardChart} from './components/home';
export default {
  name: 'Dashboard',
  components: {
    DashboardHeader,
    DashboardStats,
    DashboardChart
  }
};
</script>

<style scoped>
.dashboard {
  background: #f4f7f9;
  min-height: 100vh;
  padding: 20px;
}
</style>
