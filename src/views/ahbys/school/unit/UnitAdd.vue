<template>
  <el-dialog
    title="新增招聘会"
    :visible.sync="localVisible"
    width="600px"
    @close="handleClose"
  >
    <!-- 表单区域 -->
    <el-form
      ref="recruitForm"
      :model="form"
      :rules="rules"
      label-width="100px"
    >
      <!-- 招聘会ID -->
      <el-form-item label="招聘会ID" prop="fairId">
        <el-input
          v-model="form.fairId"
          placeholder="请输入招聘会ID"
          clearable
        />
      </el-form-item>

      <!-- 单位ID -->
      <el-form-item label="单位ID" prop="companyId">
        <el-input
          v-model="form.companyId"
          placeholder="请输入单位ID"
          clearable
        />
      </el-form-item>

      <!-- 单位名称 -->
      <el-form-item label="单位名称" prop="companyName">
        <el-input
          v-model="form.companyName"
          placeholder="请输入单位名称"
          clearable
        />
      </el-form-item>

      <!-- 场地 -->
      <el-form-item label="场地" prop="venue">
        <el-input
          v-model="form.venue"
          placeholder="请输入场地"
          clearable
        />
      </el-form-item>

      <!-- 要求 -->
      <el-form-item label="要求" prop="requirement">
        <el-input
          v-model="form.requirement"
          placeholder="请输入要求"
          clearable
        />
      </el-form-item>

      <!-- 单位简介（富文本） -->
      <el-form-item label="单位简介" prop="companyIntro">
        <!-- 富文本容器 -->
        <div
          ref="editor"
          class="editor-container"
          style="border: 1px solid #ddd; min-height: 200px;"
        />
      </el-form-item>

      <!-- 状态 -->
      <el-form-item label="状态" prop="status">
        <el-select
          v-model="form.status"
          placeholder="请选择状态"
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

      <!-- 单位行业 -->
      <el-form-item label="单位行业" prop="industry">
        <el-select
          v-model="form.industry"
          placeholder="请选择单位行业"
          clearable
        >
          <el-option
            v-for="item in industryOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>

      <!-- 平均薪资水平 -->
      <el-form-item label="平均薪资水平" prop="salaryLevel">
        <el-select
          v-model="form.salaryLevel"
          placeholder="请选择平均薪资水平"
          clearable
        >
          <el-option
            v-for="item in salaryOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>

      <!-- 招聘简章ID -->
      <el-form-item label="招聘简章ID" prop="brochureId">
        <el-input
          v-model="form.brochureId"
          placeholder="请选择招聘简章ID"
          clearable
        />
      </el-form-item>

      <!-- 参会人员（动态添加） -->
      <el-form-item label="参会人员" prop="attendees">
        <div
          v-for="(attendee, index) in form.attendees"
          :key="index"
          style="display: flex; align-items: center; margin-bottom: 8px;"
        >
          <el-input
            v-model="attendee.name"
            placeholder="请输入参会人员姓名"
            clearable
            style="width: 70%; margin-right: 10px;"
          />
          <el-button
            icon="el-icon-minus"
            type="danger"
            @click="removeAttendee(index)"
            :disabled="form.attendees.length <= 1"
          />
        </div>
        <el-button
          icon="el-icon-plus"
          type="primary"
          @click="addAttendee"
          style="margin-top: 8px;"
        >
          添加参会人员
        </el-button>
      </el-form-item>

      <!-- 参会校友（动态添加） -->
      <el-form-item label="参会校友" prop="alumni">
        <div
          v-for="(alumnus, index) in form.alumni"
          :key="index"
          style="display: flex; align-items: center; margin-bottom: 8px;"
        >
          <el-input
            v-model="alumnus.name"
            placeholder="请输入参会校友姓名"
            clearable
            style="width: 70%; margin-right: 10px;"
          />
          <el-button
            icon="el-icon-minus"
            type="danger"
            @click="removeAlumnus(index)"
            :disabled="form.alumni.length <= 1"
          />
        </div>
        <el-button
          icon="el-icon-plus"
          type="primary"
          @click="addAlumnus"
          style="margin-top: 8px;"
        >
          添加参会校友
        </el-button>
      </el-form-item>
    </el-form>

    <!-- 底部按钮 -->
    <div slot="footer" class="dialog-footer">
      <el-button @click="handleClose">取消</el-button>
      <el-button type="primary" @click="submitForm">确定</el-button>
    </div>
  </el-dialog>
</template>

<script>
// 引入富文本编辑器（以 wangEditor 为例，需先安装：npm i wangeditor）
import E from 'wangeditor'
import { Message } from 'element-ui'

export default {
  name: 'RecruitDialog',
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
    formData: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      // 弹窗显隐状态
      //dialogVisible: false,
      localVisible: this.visible,  // 本地状态，接管显隐控制
      // 表单数据
      form: {
        fairId: '',          // 招聘会ID
        companyId: '',       // 单位ID
        companyName: '',     // 单位名称
        venue: '',           // 场地
        requirement: '',     // 要求
        companyIntro: '',    // 单位简介（富文本）
        status: '',          // 状态
        industry: '',        // 单位行业
        salaryLevel: '',     // 平均薪资水平
        brochureId: '',      // 招聘简章ID
        attendees: [{ name: '' }], // 参会人员（动态）
        alumni: [{ name: '' }]      // 参会校友（动态）
      },
      // 下拉选项
      statusOptions: [
        { label: '待发布', value: 'pending' },
        { label: '已发布', value: 'published' },
        { label: '已结束', value: 'closed' }
      ],
      industryOptions: [
        { label: '互联网', value: 'internet' },
        { label: '金融', value: 'finance' },
        { label: '教育', value: 'education' }
      ],
      salaryOptions: [
        { label: '5k以下', value: 'low' },
        { label: '5k-10k', value: 'medium' },
        { label: '10k以上', value: 'high' }
      ],
      // 表单校验规则
      rules: {
        fairId: [
          { required: true, message: '请输入招聘会ID', trigger: 'blur' }
        ],
        companyId: [
          { required: true, message: '请输入单位ID', trigger: 'blur' }
        ],
        companyName: [
          { required: true, message: '请输入单位名称', trigger: 'blur' }
        ],
        venue: [
          { required: true, message: '请输入场地', trigger: 'blur' }
        ],
        status: [
          { required: true, message: '请选择状态', trigger: 'change' }
        ],
        industry: [
          { required: true, message: '请选择单位行业', trigger: 'change' }
        ],
        salaryLevel: [
          { required: true, message: '请选择平均薪资水平', trigger: 'change' }
        ],
        attendees: [
          {
            validator: (rule, value, callback) => {
              const hasValid = value.some(item => item.name.trim())
              if (!hasValid) {
                callback(new Error('请至少添加一个有效的参会人员'))
              } else {
                callback()
              }
            },
            trigger: 'change'
          }
        ],
        alumni: [
          {
            validator: (rule, value, callback) => {
              const hasValid = value.some(item => item.name.trim())
              if (!hasValid) {
                callback(new Error('请至少添加一个有效的参会校友'))
              } else {
                callback()
              }
            },
            trigger: 'change'
          }
        ]
      },
      // 富文本编辑器实例
      editor: null
    }
  },
  watch: {
    // 监听弹窗显隐，初始化/销毁富文本编辑器
    visible(val) {
      // if (val) {
      //   this.initEditor()
      // } else {
      //   this.editor && this.editor.destroy()
      // }
      this.localVisible=val;
    },
    // 监听本地显隐状态，控制编辑器初始化/销毁
    localVisible(val) {
      // 通知父组件更新状态（保持双向绑定）
      this.$emit('update:visible', val);

      if (val) {
        // DOM 渲染完成后初始化编辑器
        this.$nextTick(() => {
          this.initEditor();
          // 编辑模式回显数据（如果需要）
          if (this.isEdit) {
            const data = { ...this.formData };
            // 处理富文本内容回显
            this.editor && this.editor.txt.html(data.companyIntro);
          }
        });
      } else {
        // 关闭时销毁编辑器，释放内存
        if (this.editor) {
          this.editor.destroy();
          this.editor = null;
        }
      }
    }
  },
  methods: {
    initEditor() {
      // 1. 检查 $refs.editor 是否存在
      const editorRef = this.$refs.editor;
      if (!editorRef) {
        this.$message.error('编辑器容器未找到，请检查 ref 定义');
        return;
      }
      // 2. 获取真实 DOM 元素
      const editorDom = editorRef.$el || editorRef; // 兼容不同场景

      // 3. 销毁已有实例（避免重复初始化）
      if (this.editor) {
        this.editor.destroy();
        this.editor = null;
      }

      // 4. 重新初始化编辑器
      this.editor = new E(editorDom);
      this.editor.config.onchange = (html) => {
        this.form.companyIntro = html;
      };
      this.editor.create();
    },
    // 动态添加参会人员
    addAttendee() {
      this.form.attendees.push({ name: '' })
    },
    // 移除参会人员
    removeAttendee(index) {
      this.form.attendees.splice(index, 1)
    },
    // 动态添加参会校友
    addAlumnus() {
      this.form.alumni.push({ name: '' })
    },
    // 移除参会校友
    removeAlumnus(index) {
      this.form.alumni.splice(index, 1)
    },
    // 表单校验
    validateForm() {
      return new Promise((resolve, reject) => {
        this.$refs.recruitForm.validate((valid) => {
          if (valid) {
            resolve()
          } else {
            reject(new Error('表单校验失败'))
          }
        })
      })
    },
    // 提交表单
    async submitForm() {
      try {
        await this.validateForm()
        // 这里可调用接口提交数据：await api.submitRecruit(this.form)
        Message.success('提交成功')
        this.handleClose()
      } catch (error) {
        Message.error(error.message || '提交失败')
      }
    },
    handleClose() {
      // 1. 通知父组件关闭弹窗（核心：传递关闭状态）
      //this.$emit('update:visible', false);
       this.localVisible = false
      // 2. 重置表单（可选，根据需求）
      if (this.$refs.marketForm) {
        this.$refs.marketForm.resetFields();
      }

      // 3. 重置本地数据（如动态表单、富文本等）
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

      // 4. 销毁富文本编辑器（避免内存泄漏）
      if (this.editor) {
        this.editor.destroy();
        this.editor = null;
      }
    }
  }
}
</script>

<style scoped>
/* 富文本编辑器样式 */
.editor-container {
  min-height: 200px;
}
/* 按钮样式优化 */
.dialog-footer {
  text-align: right;
}
</style>
