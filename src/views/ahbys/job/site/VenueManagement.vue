<template>
  <div class="venue-management">
    <!-- 筛选区域 -->
    <el-form
      :model="searchForm"
      inline
      label-position="right"
      class="search-form"
    >
      <el-form-item label="日期">
        <el-date-picker
          v-model="searchForm.dateRange"
          type="daterange"
          range-separator=" - "
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          @change="handleDateChange"
        />
      </el-form-item>
      <el-form-item label="场地">
        <el-select
          v-model="searchForm.venue"
          placeholder="请选择场地"
          @change="handleVenueChange"
        >
          <el-option
            v-for="(item, index) in venueOptions"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="时间段">
        <el-select
          v-model="searchForm.timeSegment"
          placeholder="请选择时间段"
          @change="handleTimeSegmentChange"
        >
          <el-option
            v-for="(item, index) in timeSegmentOptions"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="周次">
        <el-select
          v-model="searchForm.week"
          placeholder="请选择周次"
          @change="handleWeekChange"
        >
          <el-option
            v-for="(item, index) in weekOptions"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="场地状态">
        <el-select
          v-model="searchForm.venueStatus"
          placeholder="请选择预订状态"
          @change="handleVenueStatusChange"
        >
          <el-option
            v-for="(item, index) in venueStatusOptions"
            :key="index"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleSearch">搜索</el-button>
      </el-form-item>
      <el-form-item>
        <el-button @click="handleReset">重置</el-button>
      </el-form-item>
      <el-form-item>
        <el-dropdown trigger="click" ref="toolbox">
          <el-button type="primary">
            工具箱<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>预留场地</el-dropdown-item>
            <el-dropdown-item>取消预约</el-dropdown-item>
            <el-dropdown-item>增发场次</el-dropdown-item>
            <el-dropdown-item>删除场次</el-dropdown-item>
            <el-dropdown-item>导出预约表</el-dropdown-item>
            <el-dropdown-item>场地重置</el-dropdown-item>

          </el-dropdown-menu>
        </el-dropdown>
      </el-form-item>
    </el-form>

    <!-- 数据表格 -->
    <el-table
      :data="tableData"
      border
      style="width: 100%"
      class="table"
    >
      <el-table-column
        prop="date"
        label="日期"
        align="center"
      />
      <el-table-column
        prop="week"
        label="周次"
        align="center"
      />
      <el-table-column
        prop="venueName"
        label="场地名称"
        align="center"
      />
      <el-table-column
        prop="timeSegment"
        label="预约时间段"
        align="center"
      />
      <el-table-column
        prop="reserveUnit"
        label="预约单位"
        align="center"
      />
      <el-table-column
        prop="contact"
        label="联系人"
        align="center"
      />
      <el-table-column
        prop="contactPhone"
        label="联系人手机"
        align="center"
      />
      <el-table-column
        prop="reserveStatus"
        label="预约状态"
        align="center"
      />
      <el-table-column
        prop="venueStatus"
        label="场地状态"
        align="center"
      />
      <el-table-column
        prop="createTime"
        label="创建时间"
        align="center"
      />
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'VenueManagement',
  data() {
    return {
      // 搜索表单
      searchForm: {
        dateRange: null,
        venue: '',
        timeSegment: '',
        week: '',
        venueStatus: ''
      },
      // 下拉选项数据（可根据实际接口返回替换）
      venueOptions: [
        { label: '场地A', value: 'A' },
        { label: '场地B', value: 'B' },
        { label: '场地C', value: 'C' }
      ],
      timeSegmentOptions: [
        { label: '上午', value: 'morning' },
        { label: '下午', value: 'afternoon' },
        { label: '晚上', value: 'evening' }
      ],
      weekOptions: [
        { label: '第1周', value: '1' },
        { label: '第2周', value: '2' },
        { label: '第3周', value: '3' }
      ],
      venueStatusOptions: [
        { label: '已预订', value: 'reserved' },
        { label: '未预订', value: 'unreserved' },
        { label: '已取消', value: 'cancelled' }
      ],
      // 表格数据
      tableData: []
    }
  },
  methods: {
    // 日期范围改变
    handleDateChange(val) {
      this.searchForm.dateRange = val
      // 可在此直接调用搜索接口，或等点击“搜索”再调用
    },
    // 场地改变
    handleVenueChange(val) {
      this.searchForm.venue = val
    },
    // 时间段改变
    handleTimeSegmentChange(val) {
      this.searchForm.timeSegment = val
    },
    // 周次改变
    handleWeekChange(val) {
      this.searchForm.week = val
    },
    // 场地状态改变
    handleVenueStatusChange(val) {
      this.searchForm.venueStatus = val
    },
    // 搜索按钮点击
    handleSearch() {
      // 这里可根据 searchForm 内容调用接口获取数据
      // 示例：假设接口返回数据后赋值给 tableData
      // this.$api.venue.getList(this.searchForm).then(res => {
      //   this.tableData = res.data
      // }).catch(err => {
      //   console.error(err)
      // })
      // 目前模拟无数据
      this.tableData = []
    },
    // 重置按钮点击
    handleReset() {
      this.searchForm = {
        dateRange: null,
        venue: '',
        timeSegment: '',
        week: '',
        venueStatus: ''
      }
      this.tableData = []
    }
  }
}
</script>

<style scoped>
.venue-management {
  padding: 20px;
}
.search-form {
  margin-bottom: 16px;
}
.table {
  margin-top: 16px;
}
</style>
