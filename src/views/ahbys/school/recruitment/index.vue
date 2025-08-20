<template>
  <div class="recruitment-list-container">
    <!-- 搜索筛选区域 -->
    <el-form
      :inline="true"
      :model="searchForm"
      class="search-form"
    >
      <el-form-item label="关键词">
        <el-input
          v-model="searchForm.keyword"
          placeholder="请输入关键词"
          clearable
        />
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

    <!-- 招聘信息列表 -->
    <el-table
      :data="tableData"
      border
      style="width: 100%; margin-top: 20px"
    >
      <el-table-column
        label="标题"
        prop="title"
        min-width="200"
      >
        <template slot-scope="scope">
          <div class="title-info">
            <span class="main-title">{{ scope.row.title }}</span>
            <span class="create-time">创建时间：{{ scope.row.createTime }}</span>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        label="使用说明"
        prop="description"
        min-width="400"
      />
      <el-table-column
        label="操作"
        min-width="100"
      >
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="mini"
            @click="handleEdit(scope.row)"
          >编辑
          </el-button
          >
          <el-button
            type="danger"
            size="mini"
            @click="handleDelete(scope.row)"
          >删除
          </el-button
          >
        </template>
      </el-table-column>
    </el-table>

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
    <!-- 引入子组件 -->
    <AddRecruit
      :visible.sync="addRecruitVisible"
      :isEdit="isEdit"
      :edit-data="editData"
      @dialog-closed="handleClosed"
    />
  </div>
</template>



<script>
import AddRecruit from '@/views/ahbys/school/recruitment/AddRecruit.vue'

export default {
  name: 'RecruitmentList',
  components:{ AddRecruit },
  data() {
    return {
      addRecruitVisible: false,
      isEdit: false,
      editData: {},
      // 搜索表单数据
      searchForm: {
        keyword: ''
      },
      // 表格数据
      tableData: [
        {
          title: '请填写具体相关职务的名称',
          createTime: '2025-06-11 17:22:32',
          description: '请填写具体相关职务的名称请填写具体相关职务的名称请填写具体相关职'
        },
        {
          title: 'xx公司 2025秋季校园招聘',
          createTime: '2025-05-23 09:40:32',
          description: '秋季校园招聘秋季校园招聘秋季校园招聘'
        },
        {
          title: 'xx公司 2025春季校园招聘',
          createTime: '2025-05-23 09:28:14',
          description: '校园招聘校园招聘校园招聘校园招聘校园招聘校园招聘校园招聘'
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
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm)
      // 这里可调用接口，根据搜索条件请求数据，然后更新 tableData 和 pagination
      // 示例：假设接口返回新数据后更新
      // this.tableData = 新数据;
      // this.pagination.total = 新总数;
    },
    // 重置
    handleReset() {
      this.searchForm.keyword = ''
      // 可调用接口获取初始数据
    },
    // 新增
    handleAdd() {
      //this.$router.push('/recruitment/add'); // 假设跳转到新增页面，需配置对应路由
      // 或弹出新增弹窗，根据实际需求选择
      this.addRecruitVisible = true
      this.isEdit=false
    },
    // 编辑
    handleEdit(row) {
      console.log('编辑招聘信息：', row)
      // this.$router.push({
      //   path: '/recruitment/edit',
      //   query: { id: row.id } // 假设需要传递 ID，实际根据数据结构调整
      // })
      this.isEdit = true
      this.addRecruitVisible=true
      this.editData = row
    },
    // 删除
    handleDelete(row) {
      this.$confirm('此操作将永久删除该招聘信息, 是否继续?', '提示', {
        type: 'warning'
      }).then(() => {
        // 调用删除接口，成功后再更新表格数据
        const index = this.tableData.findIndex((item) => item === row)
        if (index !== -1) {
          this.tableData.splice(index, 1)
          this.pagination.total--
        }
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    // 每页条数改变
    handleSizeChange(val) {
      this.pagination.pageSize = val
      // 调用接口，根据新的页大小请求数据
    },
    // 当前页改变
    handleCurrentChange(val) {
      this.pagination.currentPage = val
      // 调用接口，根据新的页码请求数据
    },

    //关闭窗口
    handleClosed(){
      this.addRecruitVisible=false
    }
  }
}
</script>

<style scoped>
.recruitment-list-container {
  padding: 20px;
  background-color: #fff;
}

.search-form {
  margin-bottom: 10px;
}

.title-info {
  display: flex;
  flex-direction: column;
}

.main-title {
  font-weight: bold;
  margin-bottom: 4px;
}

.create-time {
  font-size: 12px;
  color: #999;
}
</style>
