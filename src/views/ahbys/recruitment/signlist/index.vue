<template>
  <div class="agreement-list">
    <!-- 搜索筛选区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="关键词">
        <el-input
          v-model="searchForm.keyword"
          placeholder="请输入姓名/院校名称/岗位名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="签约状态">
        <el-select
          v-model="searchForm.signStatus"
          placeholder="请选择签约状态"
          clearable
        >
          <el-option
            v-for="item in signStatusOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="毕业生届别">
        <el-select
          v-model="searchForm.graduateYear"
          placeholder="请选择毕业生届别"
          clearable
        >
          <el-option
            v-for="item in graduateYearOptions"
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
        <el-button type="primary" @click="handleCreate">+ 新建协议书</el-button>
      </el-form-item>
    </el-form>

    <!-- 签约协议列表表格 -->
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
        prop="inviteTime"
        label="邀约时间"
        min-width="140"
      />
      <el-table-column
        prop="signStatus"
        label="签约状态"
        min-width="120"
        :formatter="statusFormatter"
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
  name: 'AgreementList',
  data() {
    return {
      // 搜索表单数据
      searchForm: {
        keyword: '',
        signStatus: '',
        graduateYear: ''
      },
      // 签约状态下拉选项（模拟）
      signStatusOptions: [
        { label: '尚未应约', value: 'pending' },
        { label: '签约成功', value: 'success' },
        { label: '学生婉拒', value: 'reject' }
      ],
      // 毕业生届别下拉选项（模拟）
      graduateYearOptions: [
        { label: '2025届', value: '2025' },
        { label: '2024届', value: '2024' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          name: '张三',
          jobName: '前端开发工程师',
          collegeName: '安徽大学',
          gender: '男',
          education: '本科',
          phone: '13800138001',
          inviteTime: '2025-04-15 10:00:00',
          signStatus: 'pending'
        },
        {
          name: '李四',
          jobName: '后端开发工程师',
          collegeName: '合肥工业大学',
          gender: '男',
          education: '硕士',
          phone: '13800138002',
          inviteTime: '2025-04-16 14:30:00',
          signStatus: 'pending'
        },
        // 其他模拟数据...
      ],
      // 分页配置
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 6
      }
    }
  },
  methods: {
    // 签约状态格式化（带标签样式）
    statusFormatter(row) {
      const statusMap = {
        'pending': <el-tag type="info">尚未应约</el-tag>,
        'success': <el-tag type="success">签约成功，可打印协议书</el-tag>,
        'reject': <el-tag type="warning">学生婉拒</el-tag>
      }
      return statusMap[row.signStatus] || row.signStatus
    },
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm)
      // 调用接口逻辑：this.tableData = 接口返回数据; this.pagination.total = 接口返回总数
    },
    // 重置
    handleReset() {
      this.searchForm.keyword = ''
      this.searchForm.signStatus = ''
      this.searchForm.graduateYear = ''
      this.handleSearch() // 重置后重新搜索
    },
    // 新建协议书
    handleCreate() {
      this.$router.push('/agreement/create') // 跳转新建页面，需配置路由
    },
    // 查看详情
    handleDetail(row) {
      console.log('协议详情：', row)
      this.$router.push({ path: '/agreement/detail', query: { id: row.id } })
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
.agreement-list {
  padding: 20px;
  background: #fff;
}
.search-form {
  margin-bottom: 16px;
}
</style>
