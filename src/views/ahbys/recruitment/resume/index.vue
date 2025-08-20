<template>
  <div class="resume-delivery">
    <!-- 搜索筛选区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="关键词">
        <el-input
          v-model="searchForm.keyword"
          placeholder="请输入姓名/院校名称/岗位名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="招聘岗位">
        <el-select
          v-model="searchForm.job"
          placeholder="请选择招聘岗位"
          clearable
        >
          <el-option
            v-for="item in jobOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="学历要求">
        <el-select
          v-model="searchForm.education"
          placeholder="请选择学历要求"
          clearable
        >
          <el-option
            v-for="item in educationOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="投递时间">
        <el-date-picker
          v-model="searchForm.deliveryTime"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          clearable
        />
      </el-form-item>
      <el-form-item label="处理状态">
        <el-select
          v-model="searchForm.status"
          placeholder="请选择处理状态"
          clearable
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
    </el-form>

    <!-- 简历投递列表表格 -->
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
        prop="major"
        label="专业名称"
        min-width="120"
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
        prop="remark"
        label="备注"
        min-width="160"
      />
      <el-table-column
        prop="status"
        label="处理状态"
        min-width="100"
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
  name: 'ResumeDelivery',
  data() {
    return {
      // 搜索表单数据
      searchForm: {
        keyword: '',
        job: '',
        education: '',
        deliveryTime: [],
        status: ''
      },
      // 下拉选项（模拟）
      jobOptions: [
        { label: '前端开发工程师', value: 'frontend' },
        { label: '后端开发工程师', value: 'backend' }
      ],
      educationOptions: [
        { label: '本科', value: 'undergrad' },
        { label: '硕士', value: 'master' }
      ],
      statusOptions: [
        { label: '待处理', value: 'pending' },
        { label: '尚未查看', value: 'unviewed' },
        { label: '简历已读', value: 'viewed' },
        { label: '待面试名单', value: 'interview' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          name: '张三',
          jobName: '前端开发工程师',
          collegeName: '安徽大学',
          gender: '男',
          education: '本科',
          major: '计算机科学与技术',
          phone: '13800138001',
          deliveryTime: '2025-04-15',
          remark: '有React和Vue项…',
          status: 'pending'
        },
        {
          name: '李四',
          jobName: '后端开发工程师',
          collegeName: '合肥工业大学',
          gender: '男',
          education: '硕士',
          major: '软件工程',
          phone: '13800138002',
          deliveryTime: '2025-04-16',
          remark: '熟悉Java、Sprin…',
          status: 'unviewed'
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
    // 处理状态格式化（带颜色标签）
    statusFormatter(row) {
      const statusMap = {
        'pending': <el-tag type="warning">待处理</el-tag>,
        'unviewed': <el-tag type="info">尚未查看</el-tag>,
        'viewed': <el-tag type="success">简历已读</el-tag>,
        'interview': <el-tag type="primary">待面试名单</el-tag>
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
      this.searchForm = {
        keyword: '',
        job: '',
        education: '',
        deliveryTime: [],
        status: ''
      }
      this.handleSearch() // 重置后重新搜索
    },
    // 查看详情
    handleDetail(row) {
      console.log('简历详情：', row)
      this.$router.push({ path: '/delivery/detail', query: { id: row.id } })
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
.resume-delivery {
  padding: 20px;
  background: #fff;
}
.search-form {
  margin-bottom: 16px;
}
</style>
