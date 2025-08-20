<template>
  <el-dialog
    :title="this.title"
    :visible.sync="localDialogVisible"
    width="1000px"
    @close="handleClose"
  >
    <!-- 表单区域 -->
    <el-form
      ref="marketForm"
      :model="form"
      label-width="100px"
      :rules="rules"
    >
      <el-row :gutter="30" class="form-row">
        <!-- 市场类型 -->
        <el-col :span="12">
          <el-form-item label="市场类型" prop="marketType">
            <el-select
              v-model="form.marketType"
              placeholder="请选择市场类型"
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
        </el-col>
        <el-col :span="12">
          <!-- 市场主题 -->
          <el-form-item label="市场主题" prop="marketTheme">
            <el-input
              v-model="form.marketTheme"
              placeholder="请输入市场主题"
              clearable
            />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row :gutter="30" class="form-row">
        <!-- 举办方式 -->
        <el-col :span="12">
          <el-form-item label="举办方式" prop="holdMode">
            <el-select
              v-model="form.holdMode"
              placeholder="请选择举办方式"
              clearable
            >
              <el-option
                v-for="item in holdModeOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
        </el-col>
        <!-- 举办日期 -->
        <el-col :span="12">
          <el-form-item label="举办日期" prop="holdDate">
            <el-date-picker
              v-model="form.holdDate"
              type="daterange"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              value-format="yyyy-MM-dd"
              clearable
            />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row :gutter="30" class="form-row">
      <!-- 举办场地（动态添加） -->
        <el-col :span="12">
          <!-- 最大报名单位数 -->
          <el-form-item label="最大报名单位数" prop="maxUnits">
            <el-input
              v-model="form.maxUnits"
              placeholder="请输入最大报名单位数（0表示不受限制）"
              clearable
            />
          </el-form-item>
        </el-col>
      <!-- 报名日期 -->
        <el-col :span="12">
      <el-form-item label="报名日期" prop="registerDate">
        <el-date-picker
          v-model="form.registerDate"
          type="daterange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          value-format="yyyy-MM-dd"
          clearable
        />
      </el-form-item>
        </el-col>
      </el-row>
      <el-row :gutter="30" class="form-row">
      <!-- 联系人 -->
        <el-col :span="12">
      <el-form-item label="联系人" prop="contactPerson">
        <el-input
          v-model="form.contactPerson"
          placeholder="请输入联系人"
          clearable
        />
      </el-form-item>
        </el-col>
        <el-col :span="12">
      <!-- 联系电话 -->
      <el-form-item label="联系电话" prop="contactPhone">
        <el-input
          v-model="form.contactPhone"
          placeholder="请输入联系电话"
          clearable
        />
      </el-form-item>
        </el-col>
      </el-row>


      <el-form-item label="举办场地" prop="holdVenues">
        <div
          v-for="(venue, index) in form.holdVenues"
          :key="index"
          style="display: flex; align-items: center; margin-bottom: 8px;"
        >
          <el-input
            v-model="venue.name"
            placeholder="请输入场地名称"
            clearable
            style="width: 70%; margin-right: 10px;"
          />
          <el-button
            icon="el-icon-minus"
            type="danger"
            @click="removeVenue(index)"
            :disabled="form.holdVenues.length <= 1"
          />
        </div>
        <el-button
          icon="el-icon-plus"
          type="primary"
          @click="addVenue"
          style="margin-top: 8px;"
        >
          添加场地
        </el-button>
        <div style="color: #999; font-size: 12px; margin-top: 4px;">
          添加多个场地，回车或失去焦点确认
        </div>
      </el-form-item>

      <!-- 是否上传回执 -->
      <el-form-item label="是否上传回执" prop="needReceipt">
        <el-radio-group v-model="form.needReceipt">
          <el-radio label="是">是</el-radio>
          <el-radio label="否">否</el-radio>
        </el-radio-group>
      </el-form-item>

      <!-- 邀请函内容（富文本） -->
      <el-form-item label="邀请函内容" prop="invitationContent">
        <div
          ref="editor"
          style="border: 1px solid #ddd; min-height: 200px;"
        />
      </el-form-item>
    </el-form>

    <!-- 底部按钮 -->
    <div slot="footer" class="dialog-footer">
      <el-button @click="handleClose">返回</el-button>
      <el-button type="primary" @click="saveDraft">保存草稿</el-button>
      <el-button type="success" @click="publish">发布</el-button>
    </div>
  </el-dialog>
</template>

<script>
// 引入富文本编辑器（以 wangeditor 为例，需先安装：npm i wangeditor）
import E from 'wangeditor'

export default {
  name: 'EmploymentMarketDialog',
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
      // 1. 定义本地显隐状态，初始值同步自 prop
      localDialogVisible: this.visible,
      // 弹窗标题
      title: '',
      // 表单数据
      form: {
        marketType: '', // 市场类型
        marketTheme: '', // 市场主题
        holdMode: '', // 举办方式
        holdDate: [], // 举办日期（数组：[开始, 结束]）
        holdVenues: [{ name: '' }], // 举办场地（数组）
        contactPerson: '', // 联系人
        contactPhone: '', // 联系电话
        registerDate: [], // 报名日期（数组：[开始, 结束]）
        maxUnits: '', // 最大报名单位数
        needReceipt: '否', // 是否上传回执
        invitationContent: '' // 邀请函内容（富文本）
      },
      // 下拉选项
      marketTypeOptions: [
        { label: '线上市场', value: 'online' },
        { label: '线下市场', value: 'offline' }
      ],
      holdModeOptions: [
        { label: '线上举办', value: 'online' },
        { label: '线下举办', value: 'offline' },
        { label: '混合举办', value: 'hybrid' }
      ],
      // 表单校验规则
      rules: {
        marketType: [
          { required: true, message: '请选择市场类型', trigger: 'change' }
        ],
        marketTheme: [
          { required: true, message: '请输入市场主题', trigger: 'blur' }
        ],
        holdMode: [
          { required: true, message: '请选择举办方式', trigger: 'change' }
        ],
        holdDate: [
          { required: true, message: '请选择举办日期', trigger: 'change' }
        ],
        holdVenues: [
          {
            validator: (rule, value, callback) => {
              // 校验是否有至少一个有效场地
              const hasValidVenue = value.some(item => item.name.trim())
              if (!hasValidVenue) {
                callback(new Error('请至少添加一个有效的举办场地'))
              } else {
                callback()
              }
            },
            trigger: 'change'
          }
        ],
        contactPerson: [
          { required: true, message: '请输入联系人', trigger: 'blur' }
        ],
        contactPhone: [
          { required: true, message: '请输入联系电话', trigger: 'blur' },
          { pattern: /^1\d{10}$/, message: '请输入有效的手机号码', trigger: 'blur' }
        ],
        registerDate: [
          { required: true, message: '请选择报名日期', trigger: 'change' }
        ],
        maxUnits: [
          { required: true, message: '请输入最大报名单位数', trigger: 'blur' },
          { pattern: /^[0-9]*$/, message: '请输入数字', trigger: 'blur' }
        ],
        needReceipt: [
          { required: true, message: '请选择是否上传回执', trigger: 'change' }
        ],
        invitationContent: [
          { required: true, message: '请填写邀请函内容', trigger: 'blur' }
        ]
      },
      // 富文本编辑器实例
      editor: null
    }
  },
  watch: {
    // 新增：监听父组件传递的 visible，同步到本地状态
    visible(newVal) {
      this.localDialogVisible = newVal;
    },

    // 关键修复：监听 isEdit 变化，动态更新标题
    isEdit(newVal) {
      console.log("切换编辑/新增模式：", newVal);
      // 根据 isEdit 新值更新标题
      this.title = newVal ? '编辑就业市场' : '新增就业市场';

      // （可选）编辑模式切换时，同步回显/重置表单数据
      if (newVal) {
        // 编辑模式：回显 formData（处理场地格式）
        const data = { ...this.formData };
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
    // 监听本地显隐状态（假设你已按之前方案改为 localDialogVisible）
    localDialogVisible(val) {
      if (val) {
        // 关键：用 $nextTick 等待 DOM 渲染完成后再初始化编辑器
        this.$nextTick(() => {
          this.initEditor();
          // 编辑时回显数据（原逻辑保留，需处理场地格式）
          if (this.isEdit) {
            const data = { ...this.formData };
            data.holdVenues = data.holdVenues
              ? data.holdVenues.map(v => ({ name: v }))
              : [{ name: '' }];
            this.form = data;
            // 回显富文本内容（需在编辑器初始化后执行）
            this.editor && this.editor.txt.html(this.form.invitationContent);
          }
        });
      } else {
        // 关闭时销毁编辑器，避免内存泄漏和重复初始化
        if (this.editor) {
          this.editor.destroy();
          this.editor = null; // 重置编辑器实例，避免二次销毁报错
        }
      }
    }
  },
  mounted() {
    // 初始化编辑器（若需要默认值可在此处理）
  },
  methods: {
    // 添加举办场地
    addVenue() {
      this.form.holdVenues.push({ name: '' })  // 添加对象元素
    },
    // 删除举办场地
    removeVenue(index) {
      this.form.holdVenues.splice(index, 1)
    },
    // 提交前格式化数据（可选，根据后端需求调整）
    formatFormData() {
      const formData = { ...this.form }
      // 转换为字符串数组，如 ["场地1", "场地2"]
      formData.holdVenues = formData.holdVenues
        .map(venue => venue.name.trim())
        .filter(Boolean)  // 过滤空值
      return formData
    },
    // 初始化富文本编辑器（增加 DOM 存在性判断）
    initEditor() {
      // 1. 先判断 DOM 元素是否存在，避免传入 undefined
      const editorDom = this.$refs.editor;
      if (!editorDom) {
        this.$message.error('编辑器容器不存在，无法初始化');
        return;
      }

      // 2. 若已有编辑器实例，先销毁再重新创建（避免重复初始化）
      if (this.editor) {
        this.editor.destroy();
        this.editor = null;
      }

      // 3. 正常初始化编辑器
      this.editor = new E(editorDom); // 传入正确的 DOM 元素
      // 配置编辑器（保留原逻辑）
      this.editor.config.onchange = (html) => {
        this.form.invitationContent = html;
      };
      // 可选：配置工具栏（避免不必要的功能，减少报错概率）
      this.editor.config.menus = [
        'head', 'bold', 'fontSize', 'fontName', 'italic',
        'underline', 'strikeThrough', 'foreColor', 'backColor',
        'link', 'list', 'justify', 'quote', 'image'
      ];
      // 4. 创建编辑器
      this.editor.create();
    },
    // 表单校验
    validateForm() {
      return new Promise((resolve, reject) => {
        this.$refs.marketForm.validate((valid) => {
          if (valid) {
            resolve()
          } else {
            reject(new Error('表单校验失败'))
          }
        })
      })
    },
    // 保存草稿
    async saveDraft() {
      try {
        await this.validateForm()
        // 调用保存草稿接口（示例）
        // const res = await this.$api.market.saveDraft(this.form)
        this.$message.success('草稿保存成功')
        this.handleClose()
        // 通知父组件刷新列表
        this.$emit('saveDraft', this.form)
      } catch (error) {
        this.$message.error(error.message || '保存草稿失败')
      }
    },
    // 发布
    async publish() {
      try {
        await this.validateForm()
        // 调用发布接口（示例）
        // const res = await this.$api.market.publish(this.form)
        this.$message.success('发布成功')
        this.handleClose()
        // 通知父组件刷新列表
        this.$emit('publish', this.form)
      } catch (error) {
        this.$message.error(error.message || '发布失败')
      }
    },
    // 关闭弹窗
    handleClose() {
      this.$emit('update:visible', false)
      // 重置表单：holdVenues 改为 [{ name: '' }]，避免下次打开无初始场地
      this.form = {
        marketType: '',
        marketTheme: '',
        holdMode: '',
        holdDate: [],
        holdVenues: [{ name: '' }],  // 修复：从 [] 改为 [{ name: '' }]
        contactPerson: '',
        contactPhone: '',
        registerDate: [],
        maxUnits: '',
        needReceipt: '否',
        invitationContent: ''
      }
    }
  }
}
</script>

<style scoped>
/* 富文本编辑器样式（可自定义） */
.w-e-text-container {
  min-height: 200px;
}

/* 按钮样式优化 */
.dialog-footer {
  text-align: right;
}
</style>
