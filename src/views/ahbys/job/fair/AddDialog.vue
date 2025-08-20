<template>
  <el-dialog
    title="新增宣讲会"
    :visible.sync="visible"
    width="700px"
    :close-on-click-modal="false"
  >
    <el-form
      :model="form"
      :rules="rules"
      ref="formRef"
      label-width="120px"
      class="add-form"
      :disabled="isView"
    >
      <!-- 第一行：单位ID + 单位名称 -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="单位ID" prop="unitId">
            <el-input v-model="form.unitId" placeholder="请输入单位ID"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="单位名称" prop="unitName">
            <el-input v-model="form.unitName" placeholder="请输入单位名称"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第二行：主题 + 场次ID -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="主题" prop="theme">
            <el-input v-model="form.theme" placeholder="请输入主题"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="场次ID" prop="sessionId">
            <el-input v-model="form.sessionId" placeholder="请输入场次ID"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第三行：日期 + 场地ID -->
      <el-row :gutter="30" class="form-row ">
        <el-col :span="12">
          <el-form-item label="日期" prop="date">
            <el-date-picker
              v-model="form.date"
              type="date"
              placeholder="选择日期"
              style="width: 100%"
            ></el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="场地ID" prop="venueId">
            <el-input v-model="form.venueId" placeholder="请输入场地ID"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第四行：场地名称 + 场次时间段ID -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="场地名称" prop="venueName">
            <el-input v-model="form.venueName" placeholder="请输入场地名称"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="场次时间段ID" prop="sessionTimeId">
            <el-input v-model="form.sessionTimeId" placeholder="请输入场次时间段ID"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第五行：场次时间段 + BOOKINGINTO -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="场次时间段" prop="sessionTime">
            <el-input v-model="form.sessionTime" placeholder="请输入场次时间段"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="BOOKINGINTO" prop="bookingInto">
            <el-input v-model="form.bookingInto" placeholder="请输入BOOKINGINTO"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第六行：联系人 + 联系电话 -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="联系人" prop="contact">
            <el-input v-model="form.contact" placeholder="请输入联系人"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="联系电话" prop="contactTel">
            <el-input v-model="form.contactTel" placeholder="请输入联系电话"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第七行：手机号 + 其他要求 -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="手机号" prop="phone">
            <el-input v-model="form.phone" placeholder="请输入手机号"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="其他要求" prop="otherReq">
            <el-input v-model="form.otherReq" placeholder="请输入其他要求"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第八行：宣讲文案（富文本，占满一行）
      <el-row :gutter="30" class="form-row">
        <el-col :span="24">
          <el-form-item label="宣讲文案" prop="speechContent">
      这里用 wangeditor 示例，也可替换成其他富文本 -
            <div style="border: 1px solid #ccc; min-height: 300px;">
              <wang-editor
                v-model="form.speechContent"
                :config="editorConfig"
                @onChange="handleEditorChange"
              />
            </div>
          </el-form-item>
        </el-col>
      </el-row>
      -->
      <!-- 第九行：预约状态 + 预约时间 -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="预约状态" prop="reserveStatus">
            <el-select v-model="form.reserveStatus" placeholder="请选择预约状态">
              <el-option
                v-for="item in reserveStatusOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="预约时间" prop="reserveTime">
            <el-date-picker
              v-model="form.reserveTime"
              type="datetime"
              placeholder="选择预约时间"
              style="width: 100%"
            ></el-date-picker>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第十行：宣讲方式 + 直播平台 -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="宣讲方式" prop="speechMode">
            <el-select v-model="form.speechMode" placeholder="请选择宣讲方式">
              <el-option
                v-for="item in speechModeOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="直播平台" prop="livePlatform">
            <el-input v-model="form.livePlatform" placeholder="请输入直播平台"></el-input>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第十一行：直播链接 + 薪资水平 -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="直播链接" prop="liveLink">
            <el-input v-model="form.liveLink" placeholder="请输入直播链接"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="薪资水平" prop="salaryLevel">
            <el-select v-model="form.salaryLevel" placeholder="请选择薪资水平">
              <el-option
                v-for="item in salaryLevelOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
        </el-col>
      </el-row>

      <!-- 第十二行：招聘简章ID -->
      <el-row :gutter="30" class="form-row">
        <el-col :span="12">
          <el-form-item label="招聘简章ID" prop="recruitId">
            <el-input v-model="form.recruitId" placeholder="请输入招聘简章ID"></el-input>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <!-- 可留空或扩展其他字段 -->
        </el-col>
      </el-row>
    </el-form>

    <div slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="handleSubmit">
        {{ isEdit ? '保存编辑' : '确定新增' }}
      </el-button>
    </div>
  </el-dialog>
</template>

<script>
// 引入 wangeditor 相关依赖（若用其他富文本需替换）
// import WangEditor from '@wangeditor/editor-for-vue2'
// import '@wangeditor/editor/dist/css/style.css'
// import { Editor, Toolbar } from '@wangeditor/editor'

export default {
  // components: {
  //   WangEditor
  // },
  name: 'AddDialog',
  props: {
    // 接收父组件传递的编辑数据
    editData: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      visible: false,
      isEdit: false, // 标记是否为编辑状态
      isView: false,    // 查看状态
      form: {
        unitId: '',
        unitName: '',
        theme: '',
        sessionId: '',
        date: null,
        venueId: '',
        venueName: '',
        sessionTimeId: '',
        sessionTime: '',
        bookingInto: '',
        contact: '',
        contactTel: '',
        phone: '',
        otherReq: '',
        speechContent: '',
        reserveStatus: '',
        reserveTime: null,
        speechMode: '',
        livePlatform: '',
        liveLink: '',
        salaryLevel: '',
        recruitId: ''
      },
      rules: {
        unitId: [{ required: true, message: '请输入单位ID', trigger: 'blur' }],
        unitName: [{ required: true, message: '请输入单位名称', trigger: 'blur' }],
        theme: [{ required: true, message: '请输入主题', trigger: 'blur' }],
        sessionId: [{ required: true, message: '请输入场次ID', trigger: 'blur' }],
        date: [{ required: true, message: '请选择日期', trigger: 'change' }],
        venueId: [{ required: true, message: '请输入场地ID', trigger: 'blur' }],
        venueName: [{ required: true, message: '请输入场地名称', trigger: 'blur' }],
        sessionTimeId: [{ required: true, message: '请输入场次时间段ID', trigger: 'blur' }],
        sessionTime: [{ required: true, message: '请输入场次时间段', trigger: 'blur' }],
        bookingInto: [{ required: true, message: '请输入BOOKINGINTO', trigger: 'blur' }],
        contact: [{ required: true, message: '请输入联系人', trigger: 'blur' }],
        contactTel: [{ required: true, message: '请输入联系电话', trigger: 'blur' }],
        phone: [{ required: true, message: '请输入手机号', trigger: 'blur' }],
        otherReq: [{ required: true, message: '请输入其他要求', trigger: 'blur' }],
        speechContent: [{ required: true, message: '请输入宣讲文案', trigger: 'blur' }],
        reserveStatus: [{ required: true, message: '请选择预约状态', trigger: 'change' }],
        reserveTime: [{ required: true, message: '请选择预约时间', trigger: 'change' }],
        speechMode: [{ required: true, message: '请选择宣讲方式', trigger: 'change' }],
        livePlatform: [{ required: true, message: '请输入直播平台', trigger: 'blur' }],
        liveLink: [{ required: true, message: '请输入直播链接', trigger: 'blur' }],
        salaryLevel: [{ required: true, message: '请选择薪资水平', trigger: 'change' }],
        recruitId: [{ required: true, message: '请输入招聘简章ID', trigger: 'blur' }]
      },
      // 下拉选项数据
      reserveStatusOptions: [
        { label: '已预约', value: 'reserved' },
        { label: '未预约', value: 'unreserved' },
        { label: '已取消', value: 'cancelled' }
      ],
      speechModeOptions: [
        { label: '线下', value: 'offline' },
        { label: '线上', value: 'online' },
        { label: '混合', value: 'hybrid' }
      ],
      salaryLevelOptions: [
        { label: '5k-10k', value: '5-10' },
        { label: '10k-20k', value: '10-20' },
        { label: '20k+', value: '20+' }
      ],
      // 富文本编辑器配置
      editorConfig: {
        placeholder: '请输入宣讲文案内容...',
        MENU_CONF: {
          // 可配置上传图片、视频等功能，这里简单示例
          uploadImage: {
            // 实际需替换成后端上传接口
            server: '/upload-image',
            maxFileSize: 2 * 1024 * 1024 // 2M
          }
        }
      }
    }
  },
  computed: {
    // 根据状态动态显示标题
    dialogTitle() {
      if (this.isView) return '查看宣讲会'
      if (this.isEdit) return '编辑宣讲会'
      return '新增宣讲会'
    }
  },
  watch: {
    // 监听 editData 变化，回显数据
    editData: {
      deep: true,
      handler(newVal) {
        if (newVal && Object.keys(newVal).length > 0) {
          this.isEdit = true;
          this.form = { ...newVal };
          // 富文本内容单独处理（如果需要）
          this.form.speechContent = newVal.speechContent || '';
        }
      }
    }
  },
  methods: {
   // handleEditorChange(editor) {
      // 实时更新表单数据（wangeditor 需手动同步）
    //  this.form.speechContent = editor.getHtml()
   // },
    // 打开弹窗的方法：支持新增(isEdit=false)、编辑(isEdit=true)、查看(isView=true)
    open(mode = 'add', data = {}) {
      this.isEdit = mode === 'edit'
      this.isView = mode === 'view'
      this.visible = true
      this.form = { ...data }

      this.$nextTick(() => {
        if (this.$refs.formRef) {
          this.$refs.formRef.resetFields()
        }
      })
    },
    handleSubmit() {
      if (!this.$refs.formRef) return
      this.$refs.formRef.validate((valid) => {
        if (valid) {
          this.$emit('save', { ...this.form })
          this.visible = false
        }
      })
    }
  },
  beforeDestroy() {
    // 销毁富文本实例，避免内存泄漏
    const editor = Editor.getEditor(this.$refs.wangEditor)
    if (editor) {
      editor.destroy()
    }
  }
}
</script>

<style scoped>

.el-form-item {
  width: 100%;
}
.dialog-footer {
  text-align: right;
}
/* 富文本编辑器最小高度，可根据需求调整
.wang-editor-container {
  min-height: 300px;
}
*/

/* 隐藏日期选择器的图标 */
::v-deep .el-date-editor .el-input__icon {
  display: none;
}

</style>
