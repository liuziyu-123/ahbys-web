<template>
  <!-- visible.sync 控制弹窗显隐 -->
  <el-dialog  :title="isEdit ? '编辑场地' : '新增场地'" :visible.sync="visible" width="500px">
    <el-form ref="siteform"      :model="form" label-width="80px" :rules="siteRules">
      <!-- 场地名称 -->
      <el-form-item label="场地名称" prop="name">
        <el-input
          v-model="form.name"
          placeholder="请输入场地名称"
        />
      </el-form-item>
      <!-- 容纳人数 -->
      <el-form-item label="容纳人数" prop="capacity">
        <el-input
          v-model="form.capacity"
          placeholder="请输入容纳人数"
          type="number"
        />
      </el-form-item>
      <!-- 场地备注 -->
      <el-form-item label="场地备注" prop="remark">
        <el-input
          v-model="form.remark"
          placeholder="请输入场地备注"
        />
      </el-form-item>
      <!-- 场地类型（单选） -->
      <el-form-item label="场地类型" prop="type">
        <el-radio-group v-model="form.type">
          <el-radio label="固定场地">固定场地</el-radio>
          <el-radio label="非固定场地">非固定场地</el-radio>
        </el-radio-group>
      </el-form-item>
      <!-- 场地用途（单选） -->
      <el-form-item label="场地用途" prop="use">
        <el-radio-group v-model="form.use">
          <el-radio label="线下宣讲">线下宣讲</el-radio>
          <el-radio label="直播宣讲">直播宣讲</el-radio>
          <el-radio label="虚拟场地">虚拟场地</el-radio>
        </el-radio-group>
      </el-form-item>
      <!-- 预约状态（单选） -->
      <el-form-item label="预约状态" prop="reserveStatus">
        <el-radio-group v-model="form.reserveStatus">
          <el-radio label="可预约">可预约</el-radio>
          <el-radio label="不可预约">不可预约</el-radio>
        </el-radio-group>
      </el-form-item>
    </el-form>

    <!-- 底部按钮 -->
    <div slot="footer" class="dialog-footer">
      <el-button @click="visible  = false">取消</el-button>
      <el-button type="primary" @click="handleConfirm">确定</el-button>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: 'SiteDialog',
  // 关键：接收父组件传递的 dialog-visible 属性
  props: {
    dialogVisible: {
      type: Boolean,
      default: false
    },
    formData: { type: Object, default: () => ({}) },
    isEdit: { type: Boolean, default: false }
  },
  data() {
    return {
      form: {
        name: '',
        capacity: '',
        remark: '',
        type: '',        // 固定场地/非固定场地
        use: '',         // 线下宣讲/直播宣讲/虚拟场地
        reserveStatus: ''// 可预约/不可预约
      },
      siteRules: {
        //场地名称
        name: [
          { required: true, trigger: 'blur', message: '请输入场地名称' },
        ],
        //容纳人数
        capacity: [
          { required: true, message: '请输入容纳人数', trigger: 'blur' },
        ],
        //场地备注
        remark: [
          { required: true, message: '请输入场地备注', trigger: 'blur' },
        ],
        //场地类型
        type: [
          { required: true, message: '请输入场地类型', trigger: 'blur' },
        ],
        //场地用途
        use: [
          { required: true, message: '请输入场地用途', trigger: 'blur' },
        ],
        //预约状态
        reserveStatus: [
          { required: true, trigger: 'blur', message: '请输入预约状态' },
        ],
      },
    }
  },
  watch: {
    // 监听formData变化，同步到弹窗表单（编辑时回显）
    formData: {
      handler(val) {
        this.form = { ...val }  // 深拷贝避免双向绑定冲突
      },
      immediate: true  // 初始化时立即执行
    }
  },
  computed: {
    // 通过计算属性实现双向绑定（sync 语法糖的本质）
    visible: {
      get() {
        console.log("this.dialogVisible",this.dialogVisible)
        return this.dialogVisible;
      },
      set(val) {
        console.log("this.dialogVisible",this.dialogVisible)
        // 当弹窗关闭时，通知父组件更新 dialogVisible
        this.$emit('update:dialogVisible', val);
      }
    }
  },
  methods: {
    // 确定按钮逻辑
    handleConfirm() {
      // 简单校验（可扩展）
      if (!this.form.name) {
        this.$message.warning('请输入场地名称')
        return
      }
      if (!this.form.capacity) {
        this.$message.warning('请输入容纳人数')
        return
      }

      // 调用新增接口（示例）
      // this.$api.venue.add(this.form).then(res => {
      //   this.$message.success('新增成功')
      //   this.dialogVisible = false
      //   // 通知父组件刷新表格
      //   this.$emit('refreshTable')
      // }).catch(err => {
      //   this.$message.error('新增失败：' + err.message)
      // })
    }
  }
}
</script>

<style scoped>
.dialog-footer {
  text-align: right;
}
</style>
