<template>
  <div class="recruitment-theme">
    <!-- 提示信息 -->
    <el-alert
      title="发布后需入驻高校独立审核，才能在高校就业信息网和毕业生小程序上显示"
      type="warning"
      closable
      show-icon
      style="margin-bottom: 16px"
    />

    <!-- 搜索筛选区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="主题名称">
        <el-input
          v-model="searchForm.themeName"
          placeholder="请输入主题名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="发布状态">
        <el-select
          v-model="searchForm.publishStatus"
          placeholder="请选择发布状态"
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

    <!-- 主题列表表格 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px">
      <el-table-column
        prop="themeName"
        label="主题名称"
        min-width="180"
      />
      <el-table-column
        prop="uploadTime"
        label="上传时间"
        min-width="120"
      />
      <el-table-column
        prop="remainingDays"
        label="剩余有效天数"
        min-width="120"
      />
      <el-table-column
        prop="viewCount"
        label="被浏览"
        min-width="80"
      />
      <el-table-column
        prop="publishStatus"
        label="发布状态"
        min-width="100"
        :formatter="statusFormatter"
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
          >编辑</el-button>
          <!-- 可根据需求扩展其他操作，如删除、预览等 -->
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'RecruitmentTheme',
  data() {
    return {
      // 搜索表单数据
      searchForm: {
        themeName: '',
        publishStatus: ''
      },
      // 发布状态下拉选项
      statusOptions: [
        { label: '已发布', value: '已发布' },
        { label: '已过期', value: '已过期' },
        { label: '未发布', value: '未发布' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          themeName: '2025年春季校园招聘',
          uploadTime: '2025-04-10',
          remainingDays: 25,
          viewCount: 156,
          publishStatus: '已发布'
        },
        {
          themeName: '公司简介视频',
          uploadTime: '2025-03-15',
          remainingDays: 0,
          viewCount: 89,
          publishStatus: '已过期'
        },
        {
          themeName: '技术岗位介绍',
          uploadTime: '2025-04-12',
          remainingDays: 27,
          viewCount: 42,
          publishStatus: '未发布'
        },
        {
          themeName: '员工福利与发展',
          uploadTime: '2025-04-05',
          remainingDays: 20,
          viewCount: 78,
          publishStatus: '已发布'
        },
        {
          themeName: '实习生招聘计划',
          uploadTime: '2025-04-08',
          remainingDays: 23,
          viewCount: 105,
          publishStatus: '已发布'
        }
      ]
    }
  },
  methods: {
    // 发布状态格式化（显示不同标签）
    statusFormatter(row) {
      const statusMap = {
        '已发布': <el-tag type="success">已发布</el-tag>,
        '已过期': <el-tag type="danger">已过期</el-tag>,
        '未发布': <el-tag type="info">未发布</el-tag>
      }
      return statusMap[row.publishStatus] || row.publishStatus
    },
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm)
      // 调用接口逻辑：this.tableData = 接口返回数据
    },
    // 重置
    handleReset() {
      this.searchForm.themeName = ''
      this.searchForm.publishStatus = ''
      this.handleSearch() // 重置后重新搜索
    },
    // 编辑
    handleEdit(row) {
      console.log('编辑主题：', row)
      this.$router.push({ path: '/theme/edit', query: { id: row.id } })
    }
  }
}
</script>

<style scoped>
.recruitment-theme {
  padding: 20px;
  background: #fff;
}
.search-form {
  margin-bottom: 16px;
}
</style>
