<template>
  <div class="employment-market">
    <!-- 筛选搜索区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="市场名称">
        <el-input
          v-model="searchForm.marketName"
          placeholder="请输入就业市场名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="就业市场类型">
        <el-select
          v-model="searchForm.marketType"
          placeholder="请选择就业市场类型"
          clearable
        >
          <el-option
            v-for="item in marketTypeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="举办状态">
        <el-select
          v-model="searchForm.holdStatus"
          placeholder="请选择举办状态"
          clearable
        >
          <el-option
            v-for="item in holdStatusOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="院校所在地">
        <el-select
          v-model="searchForm.schoolArea"
          placeholder="请选择所在地"
          clearable
        >
          <el-option
            v-for="item in schoolAreaOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="院校类型">
        <el-select
          v-model="searchForm.schoolType"
          placeholder="请选择院校类型"
          clearable
        >
          <el-option
            v-for="item in schoolTypeOptions"
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

    <!-- 就业市场列表 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px">
      <el-table-column
        prop="theme"
        label="就业市场主题"
        min-width="120"
      />
      <el-table-column
        prop="schoolName"
        label="院校名称"
        min-width="150"
      />
      <el-table-column
        prop="holdDate"
        label="举办日期"
        min-width="180"
      />
      <el-table-column
        prop="applyDate"
        label="报名日期"
        min-width="180"
      />
      <el-table-column
        prop="applyStatus"
        label="报名状态"
        min-width="100"
        :formatter="statusFormatter"
      />
      <el-table-column
        prop="boothNo"
        label="展位号"
        min-width="80"
      />
      <el-table-column
        label="操作"
        min-width="120"
      >
        <template slot-scope="scope">
          <el-button
            type="text"
            v-if="scope.row.applyStatus === '报名已结束'"
          >报名已结束</el-button
          >
          <el-button
            type="primary"
            size="mini"
            v-if="scope.row.applyStatus === '草稿'"
            @click="handleContinueApply(scope.row)"
          >继续报名</el-button
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
  name: 'EmploymentMarket',
  data() {
    return {
      // 搜索表单数据
      searchForm: {
        marketName: '',
        marketType: '',
        holdStatus: '',
        schoolArea: '',
        schoolType: ''
      },
      // 下拉选项（模拟，实际需接口获取）
      marketTypeOptions: [
        { label: '校园招聘会', value: 'campus' },
        { label: '人才市场', value: 'talent' }
      ],
      holdStatusOptions: [
        { label: '已举办', value: 'held' },
        { label: '未举办', value: 'unheld' }
      ],
      schoolAreaOptions: [
        { label: '安徽省', value: 'anhui' },
        { label: '江苏省', value: 'jiangsu' }
      ],
      schoolTypeOptions: [
        { label: '本科院校', value: 'undergrad' },
        { label: '专科院校', value: 'junior' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          theme: '测试',
          schoolName: '中国科学技术大学',
          holdDate: '2025-06-28 至 2025-06-28',
          applyDate: '2025-06-11 至 2025-06-21',
          applyStatus: '报名已结束',
          boothNo: ''
        },
        {
          theme: '1',
          schoolName: '中国科学技术大学',
          holdDate: '2025-06-13 至 2025-06-14',
          applyDate: '2025-06-04 至 2025-06-10',
          applyStatus: '草稿',
          boothNo: ''
        },
        {
          theme: '高校人才网组团',
          schoolName: '中国科学技术大学',
          holdDate: '2025-06-08 至 2025-06-10',
          applyDate: '2025-06-03 至 2025-06-06',
          applyStatus: '报名已结束',
          boothNo: ''
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
    // 状态格式化（根据状态显示标签）
    statusFormatter(row) {
      const statusMap = {
        '报名已结束': <el-tag type="info">报名已结束</el-tag>,
        '草稿': <el-tag type="warning">草稿</el-tag>
      }
      return statusMap[row.applyStatus] || row.applyStatus
    },
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm)
      // 调用接口逻辑：this.tableData = 接口返回数据; this.pagination.total = 接口返回总数
    },
    // 重置
    handleReset() {
      this.searchForm.marketName = ''
      this.searchForm.marketType = ''
      this.searchForm.holdStatus = ''
      this.searchForm.schoolArea = ''
      this.searchForm.schoolType = ''
      this.handleSearch() // 重置后重新搜索
    },
    // 继续报名（草稿状态时）
    handleContinueApply(row) {
      console.log('继续报名：', row)
      this.$router.push({ path: '/apply/continue', query: { id: row.id } })
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
.employment-market {
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
