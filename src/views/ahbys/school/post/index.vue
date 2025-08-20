<template>
  <div class="position-list-container">
    <!-- 搜索筛选区域 -->
    <el-form
      :inline="true"
      :model="searchForm"
      class="search-form"
    >
      <el-form-item label="关键词">
        <el-input
          v-model="searchForm.keyword"
          placeholder="请输入职位名称"
          clearable
        />
      </el-form-item>
      <el-form-item label="工作类型">
        <el-select
          v-model="searchForm.workType"
          placeholder="请选择工作类型"
          clearable
        >
          <el-option
            v-for="item in workTypeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>
      <el-form-item label="招聘状态">
        <el-select
          v-model="searchForm.recruitStatus"
          placeholder="请选择招聘状态"
          clearable
        >
          <el-option
            v-for="item in recruitStatusOptions"
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
        <el-button type="primary" @click="handleAdd">+ 新增职位</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="success" @click="handleExport">导出</el-button>
      </el-form-item>
    </el-form>

    <!-- 职位列表表格 -->
    <el-table
      :data="tableData"
      border
      style="width: 100%; margin-top: 20px"
      v-loading="loading"
    >
      <el-table-column
        prop="positionName"
        label="职位名称"
        min-width="120"
      />
      <el-table-column
        prop="positionCategory"
        label="职位类别"
        min-width="100"
      />
      <el-table-column
        prop="salary"
        label="月薪"
        min-width="100"
      />
      <el-table-column
        prop="updateTime"
        label="更新时间"
        min-width="160"
      />
      <el-table-column
        prop="endDate"
        label="截止日期"
        min-width="160"
      />
      <el-table-column
        prop="clickCount"
        label="点击次数"
        min-width="80"
      />
      <el-table-column
        prop="recruitStatus"
        label="招聘状态"
        min-width="80"
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
          <el-button
            type="danger"
            size="mini"
            @click="handleDelete(scope.row)"
          >删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <AddPost @add-success="handleAddSuccess"
             @dialog-closed="handleCloseDialog"
             :add-post-visible.sync="addPostVisible"
             :edit-data="this.editRow"
    />

    <!-- 分页组件 -->
    <pagination v-show="pagination.total>0" :total="pagination.total" :page.sync="pagination.currentPage" :limit.sync="pagination.pageSize" />
  </div>
</template>

<script>
import AddPost from './AddPost.vue'
import { SiteDialog } from '@/views/components/site'

export default {
  name: 'PositionList',
  components: {
    SiteDialog,
    AddPost
  },
  data() {
    return {
      addPostVisible: false, // 控制新增职位弹窗显示隐藏
      loading: false, // 新增加载状态
      editRow: {}, // 存储当前编辑行的数据
      // 搜索表单数据
      searchForm: {
        keyword: '',
        workType: '',
        recruitStatus: ''
      },
      // 工作类型下拉选项
      workTypeOptions: [
        { label: '全职', value: 'fulltime' },
        { label: '兼职', value: 'parttime' },
        { label: '实习', value: 'intern' }
      ],
      // 招聘状态下拉选项
      recruitStatusOptions: [
        { label: '招聘中', value: 'recruiting' },
        { label: '已结束', value: 'ended' }
      ],
      // 表格数据
      tableData: [
        {
          positionName: '请填写具体相关职务的名称',
          positionCategory: '软件工程师',
          salary: '8000-10000',
          updateTime: '2025-06-12 15:38:35',
          endDate: '2025-07-11 23:59:59',
          clickCount: 1,
          recruitStatus: 'recruiting'
        },
        {
          positionName: '中级软件工程师',
          positionCategory: '软件工程师',
          salary: '10000-15000',
          updateTime: '2025-05-23 09:47:11',
          endDate: '2025-06-23 23:59:59',
          clickCount: 1,
          recruitStatus: 'recruiting'
        },
        {
          positionName: '高级工程师',
          positionCategory: '高级软件工程师',
          salary: '15000-20000',
          updateTime: '2025-05-23 09:45:52',
          endDate: '2025-06-23 23:59:59',
          clickCount: 1,
          recruitStatus: 'recruiting'
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
    // 状态格式化函数
    statusFormatter(row) {
      return row.recruitStatus === 'recruiting' ? '招聘中' : '已结束';
    },
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm);
      this.loading = true;
      // 模拟接口请求延迟
      setTimeout(() => {
        // 这里替换为实际接口请求
        this.loading = false;
      }, 500);
    },
    // 重置
    handleReset() {
      this.searchForm.keyword = '';
      this.searchForm.workType = '';
      this.searchForm.recruitStatus = '';
      this.handleSearch();
    },
    // 新增职位
    handleAdd() {
      //this.$router.push('/position/add');
      this.addPostVisible = true;
    },
    // 导出
    handleExport() {
      console.log('执行导出操作');
    },
    // 编辑
    handleEdit(row) {
      console.log('编辑职位：', row);
      // this.$router.push({
      //   path: '/position/edit',
      //   query: { id: row.id }
      // });
      console.log('编辑数据：', row);
      this.addPostVisible = true;
      this.editRow = { ...row };
    },
    // 删除
    handleDelete(row) {
      this.$confirm('此操作将永久删除该职位, 是否继续?', '提示', {
        type: 'warning'
      }).then(() => {
        const index = this.tableData.findIndex(item => item.positionName === row.positionName);
        if (index !== -1) {
          this.tableData.splice(index, 1);
          this.pagination.total--;
        }
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    // 每页条数改变
    handleSizeChange(val) {
      this.pagination.pageSize = val;
      this.handleSearch();
    },
    // 当前页改变
    handleCurrentChange(val) {
      this.pagination.currentPage = val;
      this.handleSearch();
    },

// 1. 新增：处理子组件传递的新增数据（核心补充方法）
    handleAddSuccess(newPositionData) {
      // 给新增数据补充表格所需的默认字段（若子组件未传）
      const completeData = {
        positionCategory: newPositionData.positionType || '未分类', // 职位类别映射
        salary: newPositionData.salaryRange || '面议', // 月薪映射
        ...newPositionData
      };
      // 将新增数据插入表格最前面
      this.tableData.unshift(completeData);
      // 更新分页总数
      this.pagination.total++;
      // 提示用户
      this.$message({
        type: 'success',
        message: '职位新增成功！'
      });
      // 关闭弹窗
      this.addPostVisible = false;
    },
    // 2. 关闭弹窗：仅父组件修改 addPostVisible
    handleCloseDialog() {
      this.addPostVisible = false;
    },

  },
// 新增职位成功后回调，更新表格数据
  created() {
    // 组件创建时加载数据
    this.handleSearch();
  }
};
</script>

<style scoped>
.position-list-container {
  padding: 20px;
  background-color: #fff;
}
.search-form {
  margin-bottom: 10px;
}
</style>
