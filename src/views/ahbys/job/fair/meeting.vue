<template>
  <div class="reservation-container">
    <!-- 查询条件区域 -->
    <div class="card">
    <el-form :inline="true" :model="searchForm" class="search-form">
      <el-form-item label="主题" class="form-item">
        <el-input v-model="searchForm.title" placeholder="请输入主题" clearable></el-input>
      </el-form-item>
      <el-form-item label="场地" class="form-item">
        <el-select v-model="searchForm.place" placeholder="请选择场地" clearable >
          <el-option
            v-for="item in placeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="时间段" class="form-item">
        <el-select v-model="searchForm.timePeriod" placeholder="请选择时间段" clearable>
          <el-option
            v-for="item in timePeriodOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="预约状态" class="form-item">
        <el-select v-model="searchForm.status" placeholder="请选择预约状态" clearable>
          <el-option
            v-for="item in statusOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="创建时间" class="form-item date-item">
        <el-date-picker
          v-model="searchForm.createTime"
          type="daterange"
          range-separator="-"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        ></el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleSearch" class="form-item button-item" >搜索</el-button>
      </el-form-item>
      <el-form-item>
        <el-button @click="handleReset" class="form-item button-item">重置</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="handleAdd" class="form-item button-item">+ 新增</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="success" @click="handleExport" class="form-item button-item">导出</el-button>
      </el-form-item>
    </el-form>
    </div>
    <!-- 引入新增窗口组件 -->
    <AddDialog
      ref="addDialogRef"
      :edit-data="this.editRow"
      :place-options="placeOptions"
      :status-options="statusOptions"
      @save="handleSave"
    />
    <!-- 数据表格 -->
    <el-table :data="tableData" border style="width: 100%" class="table-list">
      <el-table-column prop="title" label="主题"></el-table-column>
      <el-table-column prop="place" label="预约场地"></el-table-column>
      <el-table-column prop="reserveTime" label="预约时间"></el-table-column>
      <el-table-column prop="contact" label="联系方式"></el-table-column>
      <el-table-column prop="status" label="预约状态"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button type="text" @click="handleView(scope.row)">查看</el-button>
          <el-button type="text" @click="handleDelete(scope.row)" style="color: red;">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页组件 -->
<!--    <el-pagination-->
<!--      @size-change="handleSizeChange"-->
<!--      @current-change="handleCurrentChange"-->
<!--      :current-page="pagination.currentPage"-->
<!--      :page-sizes="[10, 20, 50, 100]"-->
<!--      :page-size="pagination.pageSize"-->
<!--      layout="total, sizes, prev, pager, next, jumper"-->
<!--      :total="pagination.total"-->
<!--      style="margin-top: 20px; text-align: right;"-->
<!--    ></el-pagination>-->
    <pagination v-show="pagination.total>0" :total="pagination.total" :page.sync="pagination.currentPage" :limit.sync="pagination.pageSize" />
  </div>
</template>

<script>
import AddDialog from './AddDialog.vue'
export default {
  name: 'ReservationList',
  components: {
    AddDialog  // 注册新增弹窗组件
  },
  data() {
    return {
      editRow: {}, // 存储当前编辑行的数据
      // 查询表单数据
      searchForm: {
        title: '',
        place: '',
        timePeriod: '',
        status: '',
        createTime: []
      },
      // 下拉选项数据（实际可从接口获取）
      placeOptions: [
        { label: '场地A', value: 'A' },
        { label: '场地B', value: 'B' }
      ],
      timePeriodOptions: [
        { label: '上午', value: 'morning' },
        { label: '下午', value: 'afternoon' }
      ],
      statusOptions: [
        { label: '已预约', value: 'reserved' },
        { label: '已取消', value: 'cancelled' }
      ],
      // 表格数据
      tableData: [
        {
          title: '11',
          place: '11',
          reserveTime: '2025-08-13 00:00:00',
          contact: '111 11',
          status: '已预约'
        }
      ],
      // 分页数据
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 1
      }
    }
  },
  methods: {
    // 搜索
    handleSearch() {
      console.log('搜索条件：', this.searchForm);
      // 可在此调用接口，根据 searchForm 传参获取表格数据
    },
    // 重置
    handleReset() {
      this.searchForm = {
        title: '',
        place: '',
        timePeriod: '',
        status: '',
        createTime: []
      };
    },
    // 新增
    handleAdd() {
      console.log('点击新增');
      // 调用子组件的open方法打开弹窗
      this.$refs.addDialogRef.open('add')
    },
    // 导出
    handleExport() {
      console.log('点击导出');
      // 可调用导出接口，实现数据导出功能
    },
    // 编辑
    handleEdit(row) {
      console.log('编辑数据：', row);
      this.editRow = { ...row };
      // 可跳转编辑页面或弹出编辑弹窗，传递 row 数据
      this.$refs.addDialogRef.open('edit', row)
    },
    //查看
    handleView(row){
      console.log('查看数据：', row);
      this.editRow = { ...row };
      // 可跳转编辑页面或弹出编辑弹窗，传递 row 数据
      this.$refs.addDialogRef.open('view', row)
    },
    // 删除
    handleDelete(row) {
      console.log('删除数据：', row);
      this.$refs.addDialogRef.open(true, row);
      // 可调用删除接口，删除对应数据并刷新表格
    },
    // 每页条数改变
    handleSizeChange(val) {
      this.pagination.pageSize = val;
      // 可调用接口，根据新的 pageSize 重新获取数据
    },
    // 当前页改变
    handleCurrentChange(val) {
      this.pagination.currentPage = val;
      // 可调用接口，根据新的 currentPage 重新获取数据
    }
  }
}
</script>

<style scoped>
.reservation-container {
  padding: 20px;
}

.card{
  background: #fff;
  padding: 20px 30px;
  margin: 0 0 20px 0;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* 关键：为每个表单项设置右边距 */
.form-item {
  font-size: 12px !important;
  margin-right: 95px !important; /* 右侧间距50px */
}

/* 最后一个表单项移除右边距，避免整体右侧多出间距 */
.form-item:last-child {
  margin-right: 0 !important;
}


/* 专门针对日期选择器的调整 */
.date-item {
  /* 确保日期选择器容器垂直居中 */
  align-items: center;
}


/* 缩小输入框、选择框的宽度和内边距 */
/deep/ .el-input,
/deep/ .el-select {
  width: 140px !important; /* 输入框宽度缩小 */
}

/* 日期选择器宽度单独设置（通常需要更宽一点） */
/deep/ .el-date-picker {
  width: 240px !important;
}

/deep/ .el-icon-date{
  margin-bottom: 10px !important;
}

/deep/ .el-range-separator{
  margin-bottom: 15px !important;
}


/* 调整输入框内部样式 */
/deep/ .el-input__inner,
/deep/ .el-select__inner {
  font-size: 12px !important; /* 输入内容字体缩小 */
  padding: 6px 10px !important; /* 内边距缩小，使框体变小 */
  height: 30px !important; /* 输入框高度缩小 */
}

/* 调整日期选择器触发按钮样式，确保高度一致 */
/deep/ .el-date-picker__input,
/deep/ .el-date-picker__input .el-input__inner {
  height: 30px !important;
  line-height: 30px !important;
  padding: 6px 10px !important;
}

/* 调整下拉选项样式 */
/deep/ .el-select-dropdown__item {
  font-size: 12px !important;
  padding: 6px 10px !important;
}

/* 调整日期选择器样式 */
/deep/ .el-date-picker__header-label,
/deep/ .el-date-picker__time-label,
/deep/ .el-date-table td {
  font-size: 12px !important;
}

/deep/ .el-date-picker__trigger {
  height: 30px !important;
  line-height: 30px !important;
}

/* 调整标签样式 */
/deep/ .el-form-item__label {
  font-size: 12px !important;
  padding-right: 8px !important;
}

/* 调整清除按钮大小 */
/deep/ .el-input__clear,
/deep/ .el-select__clear {
  font-size: 12px !important;
}

/deep/ .el-button--medium{
  padding: 7px 15px !important;
}


.table-list {
  margin-top: 16px;
}
</style>
