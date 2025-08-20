<template>
  <div class="venue-page">
    <!-- 筛选区域 -->
    <el-form :model="searchForm" inline label-width="80px">
      <el-form-item label="场地名称">
        <el-input v-model="searchForm.name" placeholder="请输入场地名称"></el-input>
      </el-form-item>
      <el-form-item label="场地用途">
        <el-select v-model="searchForm.use" placeholder="请选择场地用途">
          <el-option
            v-for="item in useOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="预约状态">
        <el-select v-model="searchForm.status" placeholder="请选择预约状态">
          <el-option
            v-for="item in statusOptions"
            :key="item.value"
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
        <el-button type="primary" icon="el-icon-plus" @click="handleAdd">新增</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="success" icon="el-icon-download" @click="handleExport">导出</el-button>
      </el-form-item>
    </el-form>


    <!-- 数据表格 -->
    <el-table :data="tableData" border style="width: 100%" class="mt-20">
      <el-table-column prop="name" label="场地名称" align="center"></el-table-column>
      <el-table-column prop="capacity" label="容纳人数" align="center"></el-table-column>
      <el-table-column prop="type" label="场地类型" align="center">
        <template #default="scope">
          <el-tag>{{ scope.row.type }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="use" label="场地用途" align="center">
        <template #default="scope">
          <el-tag>{{ scope.row.use }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="status" label="预约状态" align="center">
        <template #default="scope">
          <el-tag>{{ scope.row.status }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="createTime" label="创建时间" align="center"></el-table-column>
      <el-table-column label="操作" align="center">
        <template #default="scope">
          <el-button type="text" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button type="text" @click="handleReserveTime(scope.row)">预约时间段</el-button>
          <el-button type="text" danger @click="handleDelete(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <pagination v-show="total>0" :total="total" :page.sync="searchForm.page" :limit.sync="searchForm.size" />
    <!-- 引入弹窗组件-->
    <SiteDialog
      :dialog-visible.sync="dialogVisible"
      :is-edit="isEdit"
      :form-data="formData"
      @refreshTable="handleRefresh"
    />

    <!-- 引入子组件 -->
    <AppointmentTimeDialog
      :visible.sync="appointmentVisible"
      siteName="场地1"
      :timeList="timeList"
      @updateTimeList="handleUpdateTimeList"
    />
  </div>
</template>

<script>
import { SiteDialog ,AppointmentTimeDialog} from '../../../components/site'
export default {
  components: { SiteDialog,AppointmentTimeDialog },
  name: 'SiteManagement',
  data() {
    return {
      dialogVisible: false,
      appointmentVisible: false,
      isEdit: false,
      timeList: [
        { timeRange: '08:00-22:00' }
      ],
      formData: {},
      searchForm: {
        name: '',
        use: '',
        status: '',
        page: 1,
        size: 10
      },
      useOptions: [
        { label: '线下宣讲', value: '线下宣讲' },
        { label: '会议', value: '会议' }
      ],
      statusOptions: [
        { label: '可预约', value: '可预约' },
        { label: '已预约', value: '已预约' }
      ],
      tableData: [
        {
          name: '场地1',
          capacity: 500,
          type: '固定场地',
          use: '线下宣讲',
          status: '可预约',
          createTime: '2025-08-15 21:08:09'
        }
      ],
      total: 1
    }
  },
  methods: {
    handleSearch() {
      // 调用接口请求数据，示例：
      // this.$api.venue.list(this.searchForm).then(res => {
      //   this.tableData = res.data.records
      //   this.total = res.data.total
      // })
    },
    handleReset() {
      this.searchForm = {
        name: '',
        use: '',
        status: '',
        page: 1,
        size: 10
      }
      this.handleSearch()
    },
    handleAdd() {
      // 打开新增弹窗或路由跳转
      //this.$router.push('/venue/add')
      this.isEdit = false
      this.dialogVisible = true
      this.formData = {  // 初始化空表单
        name: '',
        capacity: '',
        type: '',
        use: '',
        status: ''
      }
    },
    handleExport() {
      // 调用导出接口
      // this.$api.venue.export(this.searchForm)
    },
    // 编辑：打开弹窗，回显当前行数据
    handleEdit(row) {
      this.isEdit = true  // 标记为编辑模式
      // 深拷贝当前行数据，避免修改弹窗数据时直接影响表格
      this.formData = { ...row }
      this.dialogVisible = true  // 打开弹窗
    },
    handleReserveTime(row) {
      // 预约时间段逻辑
      // this.$modal.form('预约时间段', ReserveTimeForm, row).then(() => {
      //   this.handleSearch()
     // })
      this.appointmentVisible = true
    },
    handleDelete(row) {
      this.$confirm('确定删除该场地？', '提示', {
        type: 'warning'
      }).then(() => {
        // 调用删除接口
        // this.$api.venue.delete(row.id).then(() => {
        //   this.$message.success('删除成功')
        //   this.handleSearch()
        // })
      })
    },
    // 接收子组件更新的时间段列表
    handleUpdateTimeList(newList) {
      this.timeList = newList
    },

    handleSizeChange(size) {
      this.searchForm.size = size
      this.handleSearch()
    },
    handleCurrentChange(page) {
      this.searchForm.page = page
      this.handleSearch()
    },
    handleRefresh() {
      // 调用表格刷新逻辑
      this.getTableData()
    }
  }
}
</script>

<style scoped>
.venue-page {
  padding: 20px;
}
.mt-20 {
  margin-top: 20px;
}
</style>
