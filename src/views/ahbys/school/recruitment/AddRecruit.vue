<template>
  <div class="recruitment-form-modal">
    <el-dialog
      :title="title"
      :visible.sync="dialogVisible"
      width="60%"
      append-to-body
      @close="cancel"
      destroy-on-close
    >
      <el-form :model="form" :rules="rules" ref="form">
        <el-form-item label="标题" prop="title">
          <el-input v-model="form.title" placeholder="请输入标题"></el-input>
        </el-form-item>
        <el-form-item label="使用说明" prop="usage">
          <el-input
            type="textarea"
            v-model="form.usage"
            placeholder="请输入使用说明"
          ></el-input>
        </el-form-item>
        <el-form-item label="内容" prop="content">
          <!-- 这里简单用textarea模拟，实际可替换为富文本编辑器 -->
          <textarea v-model="form.content" placeholder="请输入内容"></textarea>
          <!-- 如果使用vue-quill-editor，代码示例如下 -->
          <!-- <quill-editor v-model="form.content"></quill-editor> -->
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancel">取消</el-button>
        <el-button type="primary" @click="submitForm">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'RecruitForm',
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    // 区分新增/编辑（编辑时可传入 formData）
    isEdit: {
      type: Boolean,
      default: false
    },

    // 接收父组件传递的编辑数据
    editData: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      // 子组件内部“镜像变量”，用于绑定弹窗 visible，避免直接操作 props
      dialogVisible: false,
      title:"新增招聘信息",
      form: {
        title: '',
        usage: '',
        content: ''
      },
      rules: {
        title: [
          { required: true, message: '请输入标题', trigger: 'blur' }
        ],
        usage: [
          { required: true, message: '请输入使用说明', trigger: 'blur' }
        ],
        content: [
          { required: true, message: '请输入内容', trigger: 'blur' }
        ]
      }
    }
  },
  watch: {
    // 监听父组件传递的 addPostVisible，同步到子组件内部的 dialogVisible
    visible(newVal) {
      this.dialogVisible = newVal;
      // 打开弹窗时重置表单校验
      if (newVal) {
        this.$nextTick(() => {
          this.$refs.form?.clearValidate();
        });
      }
    },
    // 关键修复：监听 isEdit 变化，动态更新标题
    isEdit(newVal) {
      console.log("切换编辑/新增模式：", newVal);
      // 根据 isEdit 新值更新标题
      this.title = newVal ? '编辑招聘信息' : '新增招聘信息';

      // （可选）编辑模式切换时，同步回显/重置表单数据
      if (newVal) {
        // 编辑模式：回显 formData（处理场地格式）
        const data = { ...this.editData };
        data.holdVenues = data.holdVenues
          ? data.holdVenues.map(v => ({ name: v }))
          : [{ name: '' }];
        this.form = data;
        // 回显富文本内容（需确保编辑器已初始化）
        this.$nextTick(() => {
          this.editor && this.editor.txt.html(this.form.invitationContent);
        });
      } else {
        // 新增模式：重置表单
        this.form = {
          marketType: '',
          marketTheme: '',
          holdMode: '',
          holdDate: [],
          holdVenues: [{ name: '' }],
          contactPerson: '',
          contactPhone: '',
          registerDate: [],
          maxUnits: '',
          needReceipt: '否',
          invitationContent: ''
        };
        // 重置编辑器内容
        this.editor && this.editor.txt.html('');
      }
    },
  },
  methods: {
    // open() {
    //   this.visible = true
    // },
    cancel() {
      this.$emit('dialog-closed');
    },
    submitForm() {
      this.$refs.form.validate((valid) => {
        if (valid) {
          // 这里可以将form数据发送到后端接口
          console.log('提交的数据：', this.form)
          this.visible = false
          this.$refs.form.resetFields()
        }
      })
    }
  }
}
</script>

<style scoped>

textarea {
  width: 100%;
  height: 200px;
  padding: 10px;
}
</style>
