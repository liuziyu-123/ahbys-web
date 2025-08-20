<template>
  <el-dialog
    :title="dialogTitle"
    :visible.sync="visible"
    width="500px"
    @close="handleClose"
    center
  >
    <!-- 弹窗标题 + 场地名称 -->
    <div slot="title">
      预约时间段管理
      <span style="margin-left: 10px; color: #999;">场地名称：{{ siteName }}</span>
    </div>

    <!-- 时间段列表 + 新增/编辑表单 -->
    <div>
      <!-- 新增/编辑表单（仅在新增或编辑时显示） -->
      <el-form
        ref="timeForm"
        :model="form"
        label-width="80px"
        v-if="isFormVisible"
        style="margin-bottom: 20px;"
      >
        <el-form-item label="时间段">
          <el-time-picker
            v-model="form.startTime"
            format="HH:mm"
            value-format="HH:mm"
            placeholder="开始时间"
            style="width: 48%; margin-right: 4%;"
          />
          <span class="time-separator">-</span>
          <el-time-picker
            v-model="form.endTime"
            format="HH:mm"
            value-format="HH:mm"
            placeholder="结束时间"
            style="width: 48%;"
          />
        </el-form-item>
      </el-form>

      <!-- 时间段列表 -->
      <el-table
        :data="timeList"
        border
        style="width: 100%"
        v-else
      >
        <el-table-column
          label="时间段"
          prop="timeRange"
          align="center"
        />
        <el-table-column
          label="操作"
          align="center"
        >
          <template slot-scope="scope">
            <el-button
              type="text"
              @click="handleEdit(scope.row)"
            >编辑</el-button>
            <el-button
              type="text"
              text-color="#f56c6c"
              @click="handleDelete(scope.row)"
            >删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 底部按钮 -->
    <div slot="footer" class="dialog-footer">
      <!-- 表单状态：显示【保存】【取消】 -->
      <template v-if="isFormVisible">
        <el-button @click="cancelForm">取消</el-button>
        <el-button type="primary" @click="confirmForm">保存</el-button>
      </template>
      <!-- 列表状态：显示【新增时间段】 -->
      <template v-else>
        <el-button type="primary" @click="handleAdd">+ 新增时间段</el-button>
      </template>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: 'AppointmentTimeDialog',
  props: {
    // 控制弹窗显隐（.sync 语法糖）
    visible: {
      type: Boolean,
      default: false
    },
    // 场地名称
    siteName: {
      type: String,
      default: '场地1'
    },
    // 已有的时间段列表（格式：[{ timeRange: '08:00-22:00' }]）
    timeList: {
      type: Array,
      default: () => [
        { timeRange: '08:00-22:00' }
      ]
    }
  },
  data() {
    return {
      // 表单显示状态（true: 新增/编辑表单；false: 列表）
      isFormVisible: false,
      // 表单数据
      form: {
        startTime: '', // 开始时间（HH:mm）
        endTime: ''    // 结束时间（HH:mm）
      },
      // 编辑时暂存的索引
      editIndex: -1
    }
  },
  computed: {
    // 弹窗标题（根据表单状态变化）
    dialogTitle() {
      return this.isFormVisible
        ? (this.editIndex === -1 ? '新增时间段' : '编辑时间段')
        : '预约时间段管理'
    }
  },
  watch: {
    // 监听 visible 关闭时重置状态
    visible(newVal) {
      if (!newVal) {
        this.resetState()
      }
    }
  },
  methods: {
    // 重置组件状态
    resetState() {
      this.isFormVisible = false
      this.form = { startTime: '', endTime: '' }
      this.editIndex = -1
      this.$refs.timeForm && this.$refs.timeForm.resetFields()
    },
    // 关闭弹窗时通知父组件
    handleClose() {
      this.resetState()
      this.$emit('update:visible', false)
    },
    // 点击【新增时间段】
    handleAdd() {
      this.isFormVisible = true
      this.editIndex = -1 // 标记为新增
      this.form = { startTime: '', endTime: '' }
    },
    // 点击【编辑】
    handleEdit(row) {
      this.isFormVisible = true
      this.editIndex = this.timeList.findIndex(item => item === row)
      // 解析时间段（拆分 08:00-22:00 为 startTime 和 endTime）
      const [start, end] = row.timeRange.split('-')
      this.form = { startTime: start, endTime: end }
    },
    // 点击【删除】
    handleDelete(row) {
      this.$confirm('此操作将永久删除该时间段，是否继续？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 过滤掉被删除的项
        const newTimeList = this.timeList.filter(item => item !== row)
        this.$emit('updateTimeList', newTimeList)
        this.$message.success('删除成功！')
      }).catch(() => {
        this.$message.info('已取消删除')
      })
    },
    // 点击【取消】（表单状态）
    cancelForm() {
      this.resetState()
    },
    // 点击【保存】（表单状态）
    confirmForm() {
      // 简单校验：开始时间和结束时间必填
      if (!this.form.startTime || !this.form.endTime) {
        this.$message.warning('请选择完整的时间段！')
        return
      }
      // 拼接时间段字符串（如：08:00-22:00）
      const timeRange = `${this.form.startTime}-${this.form.endTime}`
      // 组装新数据
      const newItem = { timeRange }

      let newTimeList = [...this.timeList]
      if (this.editIndex === -1) {
        // 新增操作
        newTimeList.push(newItem)
      } else {
        // 编辑操作
        newTimeList.splice(this.editIndex, 1, newItem)
      }

      // 通知父组件更新时间段列表
      this.$emit('updateTimeList', newTimeList)
      this.$message.success('操作成功！')
      // 重置状态回到列表
      this.resetState()
    }
  }
}
</script>

<style scoped>
/* 时间选择器之间的分隔符样式 */
.time-separator {
  display: inline-block;
  margin: 0 8px;
  color: #666;
}
/* 弹窗底部按钮对齐 */
.dialog-footer {
  text-align: right;
}
/* 时间选择器宽度优化 */
.el-time-picker {
  width: 100%;
}
</style>
