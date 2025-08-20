<template>
  <div class="resume-list">
    <!-- 搜索区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="关键词">
        <el-input
          v-model="searchForm.keyword"
          placeholder="请输入姓名/院校名称/岗位名称"
          clearable
        />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleSearch">搜索</el-button>
      </el-form-item>
      <el-form-item>
        <el-button @click="handleReset">重置</el-button>
      </el-form-item>
    </el-form>

    <!-- 简历列表表格 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px">
      <el-table-column
        prop="name"
        label="姓名"
        min-width="80"
      />
      <el-table-column
        prop="jobName"
        label="岗位名称"
        min-width="120"
      />
      <el-table-column
        prop="collegeName"
        label="院校名称"
        min-width="120"
      />
      <el-table-column
        prop="gender"
        label="性别"
        min-width="60"
      />
      <el-table-column
        prop="education"
        label="学历"
        min-width="80"
      />
      <el-table-column
        prop="phone"
        label="联系电话"
        min-width="120"
      />
      <el-table-column
        prop="deliveryTime"
        label="投递时间"
        min-width="140"
      />
      <el-table-column
        prop="resumeRemark"
        label="简历备注"
        min-width="160"
      />
      <el-table-column
        label="操作"
        min-width="80"
      >
        <template slot-scope="scope">
          <el-button
            type="text"
            @click="handleDetail(scope.row)"
          >详情</el-button
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
  </div>
</template>

<script>
export default {
  name: 'ResumeList',
  data() {
    return {
      // 搜索表单数据
      searchForm: {
        keyword: ''
      },
      // 表格数据（模拟）
      tableData: [
        {
          name: '张三',
          jobName: '前端开发工程师',
          collegeName: '安徽大学',
          gender: '男',
          education: '本科',
          phone: '13800138001',
          deliveryTime: '2025-04-15',
          resumeRemark: '有React和Vue项目经验…'
        },
        {
          name: '李四',
          jobName: '后端开发工程师',
          collegeName: '合肥工业大学',
          gender: '男',
          education: '硕士',
          phone: '13800138002',
          deliveryTime: '2025-04-16',
          resumeRemark: '熟悉Java、Spring Boot…'
        },
        // 其他模拟数据...
      ],
      // 分页配置
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 8
      }
    }
  },
  methods: {
    // 搜索
    handleSearch() {
      console.log('搜索关键词：', this.searchForm.keyword)
      // 调用接口逻辑：this.tableData = 接口返回数据; this.pagination.total = 接口返回总数
    },
    // 重置
    handleReset() {
      this.searchForm.keyword = ''
      this.handleSearch() // 重置后重新搜索
    },
    // 查看详情
    handleDetail(row) {
      console.log('简历详情：', row)
      this.$router.push({ path: '/resume/detail', query: { id: row.id } })
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
    }
  }
}
</script>

<style scoped>
.resume-list {
  padding: 20px;
  background: #fff;
}
.search-form {
  margin-bottom: 16px;
}
</style>
