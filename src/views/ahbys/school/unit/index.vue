<template>
  <div class="recruitment-management">
    <!-- 筛选区域 -->
    <el-form :model="searchForm" inline label-width="80px">
      <el-form-item label="招聘会">
        <el-select v-model="searchForm.recruitment" placeholder="请选择招聘会">
          <el-option
            v-for="item in recruitmentOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="场地">
        <el-select v-model="searchForm.venue" placeholder="请选择场地">
          <el-option
            v-for="item in venueOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="状态">
        <el-select v-model="searchForm.status" placeholder="请选择状态">
          <el-option
            v-for="item in statusOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="关键词">
        <el-input v-model="searchForm.keyword" placeholder="单位名称/联系人/电话"></el-input>
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
      <el-table-column prop="unitName" label="单位名称" align="center"></el-table-column>
      <el-table-column prop="venueName" label="场地名称" align="center"></el-table-column>
      <el-table-column prop="representative" label="参会代表" align="center"></el-table-column>
      <el-table-column prop="phone" label="联系电话" align="center"></el-table-column>
      <el-table-column prop="attendCount" label="参会人数" align="center"></el-table-column>
      <el-table-column prop="alumniCount" label="校友人数" align="center"></el-table-column>
      <el-table-column prop="auditStatus" label="审核状态" align="center"></el-table-column>
      <el-table-column prop="applyDate" label="报名日期" align="center"></el-table-column>
      <el-table-column label="操作" align="center">
        <template #default="scope">
          <el-button type="text" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button type="text" danger @click="handleDelete(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <UnitAdd
      :visible.sync="dialogVisible"
      :isEdit="false"
      @refresh="fetchData"
    />
  </div>

</template>



<script>
import UnitAdd from './UnitAdd.vue';
export default {
  components: {
    UnitAdd
  },

  name: 'RecruitmentManagement',
  data() {
    return {
      dialogVisible: false,
      searchForm: {
        recruitment: '',
        venue: '',
        status: '',
        keyword: '',
        page: 1,
        size: 10
      },
      recruitmentOptions: [
        { label: '春季招聘会', value: 'spring' },
        { label: '秋季招聘会', value: 'autumn' }
      ],
      venueOptions: [
        { label: '场地A', value: 'A' },
        { label: '场地B', value: 'B' }
      ],
      statusOptions: [
        { label: '待审核', value: 'pending' },
        { label: '已审核', value: 'approved' }
      ],
      tableData: [], // 无数据时显示“暂无数据”
      total: 0
    }
  },
  methods: {
    handleSearch() {
      // 调用接口：this.$api.recruitment.list(this.searchForm)
    },
    handleReset() {
      this.searchForm = {
        recruitment: '',
        venue: '',
        status: '',
        keyword: '',
        page: 1,
        size: 10
      }
      this.handleSearch()
    },
    handleAdd() {
      // 打开新增弹窗或路由跳转
      this.dialogVisible=true
    },
    handleExport() {
      // 调用导出接口
    },
    handleEdit(row) {
      // 编辑逻辑
    },
    handleDelete(row) {
      // 删除逻辑
    },
    fetchData() {
      // 提交成功后刷新列表
    }
  }
}
</script>

<style scoped>
.recruitment-management {
  padding: 20px;
}
.mt-20 {
  margin-top: 20px;
}
</style>
