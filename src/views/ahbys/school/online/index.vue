<template>
  <div class="application-list-container">
    <!-- 搜索筛选区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="关键词">
        <el-input
          v-model="searchForm.keyword"
          placeholder="请输入关键词"
          clearable
        />
      </el-form-item>
      <el-form-item label="申请院校">
        <el-select
          v-model="searchForm.school"
          placeholder="请选择申请院校"
          clearable
        >
          <el-option
            v-for="school in schoolOptions"
            :key="school.value"
            :label="school.label"
            :value="school.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="状态">
        <el-select
          v-model="searchForm.status"
          placeholder="请选择状态"
          clearable
        >
          <el-option
            v-for="status in statusOptions"
            :key="status.value"
            :label="status.label"
            :value="status.value"
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
        <el-button type="primary" @click="handleAdd">+ 新增</el-button>
      </el-form-item>
    </el-form>

    <!-- 申请列表表格 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px">
      <el-table-column
        prop="applySchool"
        label="申请学校"
        min-width="120"
      />
      <el-table-column
        prop="recruitmentName"
        label="招聘简章名称"
        min-width="180"
      />
      <el-table-column
        prop="applyDesc"
        label="申请说明"
        min-width="300"
      />
      <el-table-column
        prop="status"
        label="状态"
        min-width="80"
        :formatter="statusFormatter"
      />
      <el-table-column
        prop="createTime"
        label="创建时间"
        min-width="160"
      />
      <el-table-column
        label="操作"
        min-width="100"
      >
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="mini"
            @click="handleView(scope.row)"
          >查看</el-button>
          <el-button
            type="danger"
            size="mini"
            @click="handleDelete(scope.row)"
            v-if="scope.row.status!== '已删除'"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 引入子组件 -->
    <AddOnline
      :visible.sync="addOnlineVisible"
      @dialog-closed="handleClosed"
    />

    <!-- 分页组件 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagination.currentPage"
      :page-sizes="[10, 20, 50]"
      :page-size="pagination.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="pagination.total"
      style="margin-top: 20px; text-align: right"
    />
  </div>
</template>

<script>
import AddOnline from './AddOnline.vue'
export default {
  name: 'ApplicationList',
  components: {  AddOnline },
  data() {
    return {
      addOnlineVisible: false,
      //isEdit: false,
      //editData: {},
      // 搜索表单数据
      searchForm: {
        keyword: '',
        school: '',
        status: ''
      },
      // 申请院校下拉选项（模拟）
      schoolOptions: [
        { label: '安徽艺术学院', value: '安徽艺术学院' },
        { label: '黄山健康职业学院', value: '黄山健康职业学院' }
      ],
      // 状态下拉选项（模拟）
      statusOptions: [
        { label: '已发布', value: '已发布' },
        { label: '已删除', value: '已删除' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          applySchool: '安徽艺术学院',
          recruitmentName: 'xx公司 2025秋季校园招聘',
          applyDesc: '具体相关职务的名称请填写具体相关职务',
          status: '已发布',
          createTime: '2025-06-11 17:29:17'
        },
        {
          applySchool: '安徽艺术学院',
          recruitmentName: 'xx公司 2025秋季校园招聘',
          applyDesc: '1',
          status: '已发布',
          createTime: '2025-05-23 10:44:38'
        },
        {
          applySchool: '黄山健康职业学院',
          recruitmentName: 'xx公司 2025秋季校园招聘',
          applyDesc: '1111111111111111111111111111111111111111',
          status: '已删除',
          createTime: '2025-05-23 09:47:29'
        }
      ],
      // 分页配置
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 3
      }
    }
  },
  methods: {
    // 状态格式化（映射为中文或自定义显示）
    statusFormatter(row) {
      const statusMap = {
        '已发布': <el-tag type="success">已发布</el-tag>,
        '已删除': <el-tag type="danger">已删除</el-tag>
      }
      return statusMap[row.status] || row.status
    },
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm)
      // 调用接口逻辑：this.tableData = 接口返回数据; this.pagination.total = 接口返回总数
    },
    // 重置
    handleReset() {
      this.searchForm.keyword = ''
      this.searchForm.school = ''
      this.searchForm.status = ''
      this.handleSearch() // 重置后重新搜索
    },
    // 新增
    handleAdd() {
      //this.$router.push('/application/add') // 跳转新增页面，需配置路由
      this.addOnlineVisible = true
     // this.isEdit=false

    },
    // 查看
    handleView(row) {
      console.log('查看申请：', row)
      //this.$router.push({ path: '/application/view', query: { id: row.id } })
      // this.isEdit = true
      // this.addOnlineVisible=true
      // this.editData = row
    },
    // 删除
    handleDelete(row) {
      this.$confirm('确认删除该申请？删除后不可恢复', '提示', {
        type: 'warning'
      }).then(() => {
        const index = this.tableData.findIndex(item => item === row)
        if (index!== -1) {
          this.tableData.splice(index, 1)
          this.pagination.total--
          this.$message.success('删除成功')
        }
      }).catch(() => {
        this.$message.info('已取消删除')
      })
    },
    // 每页条数改变
    handleSizeChange(val) {
      this.pagination.pageSize = val
      this.handleSearch() // 切换条数后重新搜索
    },
    // 当前页改变
    handleCurrentChange(val) {
      this.pagination.currentPage = val
      this.handleSearch() // 切换页码后重新搜索
    },

    //关闭窗口
    handleClosed(){
      this.addOnlineVisible=false
    }
  }
}
</script>

<style scoped>
.application-list-container {
  padding: 20px;
  background: #fff;
}
.search-form {
  margin-bottom: 16px;
}
.el-table {
  --el-table-header-text-color: #666;
}
</style>
