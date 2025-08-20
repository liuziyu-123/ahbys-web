<template>
  <div class="calendar-filter-container">
    <!-- 筛选区域 -->
    <div class="filter-bar">
      <el-date-picker
        v-model="dateRange"
        type="daterange"
        range-separator=" - "
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        @change="handleDateChange"
      ></el-date-picker>
      <el-select
        v-model="selectedVenue"
        placeholder="请选择场地"
        @change="handleVenueChange"
      >
        <el-option
          v-for="(item, index) in venueOptions"
          :key="index"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>
    </div>

    <!-- 日历展示区域 -->
    <el-calendar v-model="currentDate">
      <template slot="dateCell" slot-scope="{ date, data }">
        <div
          class="custom-date-cell"
          @click="handleDateClick(data.day)"
        >
          <!-- 日期数字 -->
          <span class="date-num" :class="{ 'today-num': isToday(data.day) }">
            {{ data.day.split('-')[2] }}
          </span>
          <!-- 日程标题（显示在日期下方，最多1行，超出省略） -->
          <div class="schedule-title" v-if="isToday(data.day) && todaySchedule.length > 0">
            {{ todaySchedule[0].content }} <!-- 显示第一条日程标题，可根据需求调整 -->
          </div>
          <!-- 日期范围标记 -->
          <div
            v-if="isDateInRange(data.day)"
            class="range-marker"
          ></div>
        </div>
      </template>
    </el-calendar>

    <!-- 日程详情弹窗 -->
    <el-dialog
      title="日程详情"
      :visible.sync="scheduleDialogVisible"
      width="400px"
      :close-on-click-modal="false"
    >
      <div class="schedule-detail">
        <p><span class="label">日期：</span>{{ currentClickDate }}</p>
        <p><span class="label">场地：</span>{{ selectedVenue || '未选择' }}</p>
        <div class="schedule-list">
          <p class="label">日程列表：</p>
          <el-empty
            v-if="currentSchedule.length === 0"
            description="暂无日程"
          ></el-empty>
          <div class="schedule-item" v-else v-for="(item, idx) in currentSchedule" :key="idx">
            {{ idx + 1 }}. {{ item.time }} - {{ item.content }}
          </div>
        </div>
      </div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="scheduleDialogVisible = false">关闭</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'CalendarFilter',
  data() {
    return {
      dateRange: null,
      venueOptions: [
        { label: '场地A', value: 'A' },
        { label: '场地B', value: 'B' },
        { label: '场地C', value: 'C' }
      ],
      selectedVenue: '',
      currentDate: new Date(),
      filterDateRange: [],
      // 日程相关
      scheduleDialogVisible: false,
      currentClickDate: '',
      currentSchedule: [],
      // 今日日程（可替换为接口数据）
      todaySchedule: [
        { time: '09:30', content: '项目需求评审会' },
        { time: '14:00', content: '宣讲会场地布置' },
        { time: '16:30', content: '团队周会' }
      ]
    };
  },
  methods: {
    handleDateChange(val) {
      this.filterDateRange = val ? val.map(item => this.formatDate(item)) : [];
    },
    handleVenueChange(val) {
      console.log('选中场地：', val);
      // 场地筛选日程（可选）
      if (this.currentClickDate && this.isToday(this.currentClickDate)) {
        this.currentSchedule = val
          ? this.todaySchedule.filter(item => item.content.includes(val))
          : [...this.todaySchedule];
      }
    },
    formatDate(date) {
      const year = date.getFullYear();
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      const day = date.getDate().toString().padStart(2, '0');
      return `${year}-${month}-${day}`;
    },
    isToday(dateStr) {
      return dateStr === this.formatDate(new Date());
    },
    isDateInRange(dateStr) {
      if (this.filterDateRange.length !== 2) return false;
      const [start, end] = this.filterDateRange;
      return dateStr >= start && dateStr <= end;
    },
    handleDateClick(dateStr) {
      this.currentClickDate = dateStr;
      this.currentSchedule = this.isToday(dateStr) ? [...this.todaySchedule] : [];
      this.scheduleDialogVisible = true;
    }
  }
};
</script>

<style scoped>
.calendar-filter-container {
  padding: 20px;
}
.filter-bar {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  gap: 16px;
}
/* 自定义日期单元格：垂直布局（日期+标题） */
.custom-date-cell {
  position: relative;
  text-align: center;
  padding: 12px 4px; /* 增加内边距，避免内容拥挤 */
  cursor: pointer;
  min-height: 80px; /* 固定单元格高度，确保布局整齐 */
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
/* 日期数字样式 */
.date-num {
  font-size: 16px;
  font-weight: 500;
  margin-bottom: 4px; /* 日期与标题间距 */
}
/* 今日日期数字：蓝色加粗 */
.today-num {
  color: #409eff;
  font-weight: 700;
}
/* 日程标题：显示在日期下方，单行省略 */
.schedule-title {
  font-size: 12px;
  color: #606266;
  width: 100%;
  white-space: nowrap; /* 禁止换行 */
  overflow: hidden; /* 隐藏超出内容 */
  text-overflow: ellipsis; /* 超出显示省略号 */
  line-height: 1.2;
}
/* 日期范围标记（底部条） */
.range-marker {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 80%;
  height: 3px;
  background-color: #409eff;
}
/* 日程详情弹窗样式 */
.schedule-detail {
  line-height: 2;
}
.schedule-detail .label {
  color: #606266;
  font-weight: 500;
}
.schedule-list {
  margin-top: 12px;
}
.schedule-item {
  margin-top: 8px;
  color: #303133;
}
/* 隐藏日期选择器图标 */
::v-deep .el-date-editor .el-input__icon {
  display: none;
}
::v-deep .el-date-editor .el-input__inner {
  padding-left: 10px !important;
}
</style>
