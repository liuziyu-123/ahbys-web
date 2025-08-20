<template>
  <div class="reservation-list">
    <!-- 搜索筛选区域 -->
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="预约主题">
        <el-input
          v-model="searchForm.theme"
          placeholder="请输入预约主题"
          clearable
        />
      </el-form-item>
      <el-form-item label="预约状态">
        <el-select
          v-model="searchForm.status"
          placeholder="请选择预约状态"
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
      <el-form-item>
        <el-button type="primary" @click="handleAdd">+ 预约宣讲会</el-button>
      </el-form-item>
    </el-form>

    <!-- 预约列表表格 -->
    <el-table :data="tableData" border style="width: 100%; margin-top: 20px">
      <el-table-column
        prop="theme"
        label="预约主题"
        min-width="120"
      />
      <el-table-column
        prop="collegeName"
        label="院校名称"
        min-width="100"
      />
      <el-table-column
        prop="venue"
        label="预约场地"
        min-width="200"
      />
      <el-table-column
        prop="reserveDate"
        label="预约日期"
        min-width="120"
      />
      <el-table-column
        prop="contact"
        label="联系人"
        min-width="80"
      />
      <el-table-column
        prop="status"
        label="预约状态"
        min-width="100"
      />
      <el-table-column
        prop="createTime"
        label="登记时间"
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

    <AddTalks
      :visible.sync="dialogVisible"
      :isEdit="isEdit"
      :edit-form="editRow"
      @refresh="fetchData"
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
import AddTalks from './AddTalks.vue'

export default {
  name: 'ReservationList',
  components: {
    AddTalks
  },
  data() {
    return {
      dialogVisible: false,
      isEdit:false,
      editRow: {}, // 存储当前编辑行的数据
      // 搜索表单数据
      searchForm: {
        theme: '',
        status: ''
      },
      // 预约状态下拉选项（可根据实际需求扩展）
      statusOptions: [
        { label: '全部', value: '' },
        { label: '已完成', value: 'completed' },
        { label: '待处理', value: 'pending' }
      ],
      // 表格数据（模拟）
      tableData: [
        {
          theme: '2025届毕业生校园宣讲会',
          collegeName: '安徽大学',
          venue: '安徽大学就业指导中心报告厅',
          reserveDate: '2025-05-15',
          contact: '张三',
          status: '已预约',
          createTime: '2025-04-10 10:30:00'
        },
        {
          theme: '计算机专业招聘宣讲',
          collegeName: '合肥工业大学',
          venue: '合肥工业大学计算机学院报…',
          reserveDate: '2025-05-20',
          contact: '李四',
          status: '已预约',
          createTime: '2025-04-12 09:15:00'
        },
        {
          theme: '软件工程专场招聘会',
          collegeName: '安徽师范大学',
          venue: '安徽师范大学就业中心',
          reserveDate: '2025-06-01',
          contact: '王五',
          status: '已预约',
          createTime: '2025-04-15 14:20:00'
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
      // 可在此调用接口，根据搜索条件请求数据更新 tableData 和 pagination
    },
    // 重置
    handleReset() {
      this.searchForm.theme = ''
      this.searchForm.status = ''
      // 可调用接口获取初始数据
    },
    // 新增预约
    handleAdd() {
     // this.$router.push('/reservation/add') // 假设跳转到新增页面，需配置对应路由
      this.dialogVisible = true
    },
    // 编辑预约
    handleEdit(row) {
      console.log('编辑预约：', row)
      // this.$router.push({
      //   path: '/reservation/edit',
      //   query: { id: row.id } // 假设传递预约 ID，实际根据数据结构调整
      // })
      this.dialogVisible = true
      this.editRow = { ...row };
    },
    // 删除预约
    handleDelete(row) {
      this.$confirm('此操作将永久删除该预约, 是否继续?', '提示', {
        type: 'warning'
      }).then(() => {
        const index = this.tableData.findIndex(item => item === row)
        if (index!== -1) {
          this.tableData.splice(index, 1)
          this.pagination.total--
        }
        this.$message({
          type:'success',
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
    fetchData() {
      // 提交成功后刷新列表
    }
  }
}
</script>

<style scoped>
.reservation-list {
  padding: 20px;
  background-color: #fff;
}
.search-form {
  margin-bottom: 10px;
}
</style>
