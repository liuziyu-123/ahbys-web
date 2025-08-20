<template>
  <div class="market-management">
    <!-- 筛选区域 -->
    <el-form :model="searchForm" inline label-width="80px">
      <el-form-item label="市场主题">
        <el-input
          v-model="searchForm.title"
          placeholder="请输入市场主题"
        />
      </el-form-item>
      <el-form-item label="市场类型">
        <el-select
          v-model="searchForm.type"
          placeholder="请选择市场类型"
        >
          <el-option
            v-for="item in typeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="发布状态">
        <el-select
          v-model="searchForm.status"
          placeholder="请选择发布状态"
        >
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
    <el-table
      :data="tableData"
      border
      style="width: 100%"
      class="mt-20"
    >
      <el-table-column
        prop="title"
        label="市场主题"
        align="center"
      />
      <el-table-column
        prop="type"
        label="市场类型"
        align="center">
        <template #default="scope">
          <el-tag>{{ scope.row.type }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="holdDate"
        label="举办日期"
        align="center"
      />
      <el-table-column
        prop="enrollDate"
        label="报名日期"
        align="center"
      />
      <el-table-column
        prop="status"
        label="发布状态"
        align="center">
        <template #default="scope">
          <el-tag>{{ scope.row.status }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="unitCount"
        label="单位数"
        align="center"
      />
      <el-table-column
        prop="jobCount"
        label="职位数"
        align="center"
      />
      <el-table-column
        prop="positionCount"
        label="岗位数"
        align="center"
      />
      <el-table-column
        prop="enrollLimit"
        label="报名上限"
        align="center"
      />
      <el-table-column
        label="操作"
        align="center">
        <template #default="scope">
          <el-button type="text" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button type="text" danger @click="handleDelete(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
  <pagination v-show="total>0" :total="total" :page.sync="searchForm.page" :limit.sync="searchForm.size" />

    <!-- 引入子组件 -->
    <MarketAdd
      :visible.sync="dialogVisible"
      :isEdit="isEdit"
      :formData="formData"
      @saveDraft="handleSaveDraft"
      @publish="handlePublish"
    />
  </div>
</template>

<script>

import MarketAdd from './MarketAdd.vue'

export default {
  components: {
    MarketAdd
  },
  name: 'MarketManagement',
  data() {
    return {
      dialogVisible: false,
      isEdit: false,
      formData: {}, // 编辑时的表单数据
      searchForm: {
        title: '',
        type: '',
        status: '',
        page: 1,
        size: 10
      },
      // 市场类型选项（可扩展）
      typeOptions: [
        { label: '校园大型双选会', value: '校园大型双选会' }
      ],
      // 发布状态选项（可扩展）
      statusOptions: [
        { label: '草稿', value: '草稿' },
        { label: '已发布', value: '已发布' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          title: '测试',
          type: '校园大型双选会',
          holdDate: '2025-06-28',
          enrollDate: '2025-06-11 至 2025-06-21',
          status: '已发布',
          unitCount: 0,
          jobCount: 0,
          positionCount: 0,
          enrollLimit: 0
        },
        {
          title: '2',
          type: '校园大型双选会',
          holdDate: '2025-06-11',
          enrollDate: '2025-06-11 至 2025-06-12',
          status: '草稿',
          unitCount: 0,
          jobCount: 0,
          positionCount: 0,
          enrollLimit: 11
        },
        {
          title: '1',
          type: '校园大型双选会',
          holdDate: '2025-06-13',
          enrollDate: '2025-06-04 至 2025-06-10',
          status: '已发布',
          unitCount: 0,
          jobCount: 0,
          positionCount: 0,
          enrollLimit: 11
        },
        {
          title: '高校人才网组团',
          type: '校园大型双选会',
          holdDate: '2025-06-08',
          enrollDate: '2025-06-03 至 2025-06-06',
          status: '已发布',
          unitCount: 0,
          jobCount: 0,
          positionCount: 0,
          enrollLimit: 100
        }
      ],
      total: 4 // 模拟总条数
    }
  },
  methods: {
    // 搜索
    handleSearch() {
      // 调用接口请求数据，示例：
      // this.$api.market.list(this.searchForm).then(res => {
      //   this.tableData = res.data.records
      //   this.total = res.data.total
      // }).catch(err => {
      //   console.error(err)
      // })
    },
    // 重置
    handleReset() {
      this.searchForm = {
        title: '',
        type: '',
        status: '',
        page: 1,
        size: 10
      }
      this.handleSearch()
    },
    // 新增
    handleAdd() {
      // 打开新增弹窗或跳转路由
      //this.$router.push('/market/add')
      this.isEdit = false
      this.dialogVisible = true
    },
    // 导出
    handleExport() {
      // 调用导出接口
      // this.$api.market.export(this.searchForm)
    },
    // 编辑
    handleEdit(row) {
      // 打开编辑弹窗或跳转路由
      ///this.$router.push(`/market/edit/${row.id}`)
      this.isEdit = true
      this.dialogVisible = true
      this.formData=row

    },
    // 删除
    handleDelete(row) {
      this.$confirm('确定删除该市场？', '提示', {
        type: 'warning'
      }).then(() => {
        // 调用删除接口
        // this.$api.market.delete(row.id).then(() => {
        //   this.$message.success('删除成功')
        //   this.handleSearch()
        // })
      })
    },
    // 每页条数改变
    handleSizeChange(size) {
      this.searchForm.size = size
      this.handleSearch()
    },
    // 当前页改变
    handleCurrentChange(page) {
      this.searchForm.page = page
      this.handleSearch()
    },

    // 打开新增弹窗
    openDialog() {

    },
    // 打开编辑弹窗（示例）
    openEditDialog() {
      this.isEdit = true
      // 模拟接口获取编辑数据
      this.formData = {
        marketType: 'online',
        marketTheme: '春季招聘会',
        // ...其他字段
      }
      this.dialogVisible = true
    },
    // 接收保存草稿事件
    handleSaveDraft(form) {
      console.log('保存草稿：', form)
      // 调用接口保存草稿...
    },
    // 接收发布事件
    handlePublish(form) {
      console.log('发布：', form)
      // 调用接口发布...
    }
  }
}
</script>

<style scoped>
.market-management {
  padding: 20px;
}
.mt-20 {
  margin-top: 20px;
}
</style>
