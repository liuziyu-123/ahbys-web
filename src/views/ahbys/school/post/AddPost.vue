<template>
  <el-dialog
    :title="dialogTitle"
    :visible.sync="dialogVisible"
    width="700px"
    @close="handleDialogClose"
    destroy-on-close
  >
    <el-form
      ref="recruitForm"
      :model="formData"
      label-width="100px"
      class="recruit-form"
    >
      <!-- 职位名称 -->
      <el-form-item label="职位名称" prop="positionName">
        <el-input
          v-model="formData.positionName"
          placeholder="请输入职位名称"
          max-length="50"
        />
        <div class="tip">请填写具体相关职务的名称,不要填写**招聘简章或公告之类</div>
      </el-form-item>

      <!-- 职位类别 -->
      <el-form-item label="职位类别" prop="positionType">
        <el-select v-model="formData.positionType" placeholder="请选择职位类别">
          <el-option
            v-for="item in positionTypeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
        <div class="tip" v-if="!positionTypeOptions.length">
          若上列表打开为空，请点击【<a href="javascript:;" @click="handleMaintain('positionType')">职位类别</a>】维护本单位招聘职位类别列表。
        </div>
      </el-form-item>

      <!-- 工作城市 -->
      <el-form-item label="工作城市" prop="workCity">
        <el-select v-model="formData.workCity" placeholder="请选择工作城市">
          <el-option
            v-for="item in workCityOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
        <div class="tip" v-if="!workCityOptions.length || !formData.workCity">
          若上列表打开为空或当前所需工作城市不在列表中，请点击【<a href="javascript:;" @click="handleMaintain('workCity')">工作城市</a>】维护本单位工作城市列表
        </div>
      </el-form-item>

      <!-- 招聘专业 -->
      <el-form-item label="招聘专业" prop="recruitMajor">
        <el-select v-model="formData.recruitMajor" placeholder="请选择招聘专业">
          <el-option
            v-for="item in recruitMajorOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
        <div class="tip" v-if="!recruitMajorOptions.length">
          若上列表打开为空，请点击【<a href="javascript:;" @click="handleMaintain('recruitMajor')">招聘专业</a>】维护本单位招聘专业列表。
        </div>
      </el-form-item>

      <!-- 学历要求 -->
      <el-form-item label="学历要求" prop="educationReq">
        <el-select v-model="formData.educationReq" placeholder="请选择学历要求">
          <el-option
            v-for="item in educationReqOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
        <div class="tip">提示：表示最低学历要求</div>
      </el-form-item>

      <!-- 工作类型 -->
      <el-form-item label="工作类型" prop="workType">
        <el-select v-model="formData.workType" placeholder="请选择工作类型">
          <el-option
            v-for="item in workTypeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>

      <!-- 招聘人数 -->
      <el-form-item label="招聘人数" prop="recruitNum">
        <el-input-number
          v-model="formData.recruitNum"
          :min="1"
          @change="handleRecruitNumChange"
        />
      </el-form-item>

      <!-- 月薪 -->
      <el-form-item label="月薪" prop="salaryRange">
        <el-select v-model="formData.salaryRange" placeholder="请选择月薪范围">
          <el-option
            v-for="item in salaryRangeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-form-item>

      <!-- 岗位职责 -->
      <el-form-item label="岗位职责" prop="jobDuty">
        <el-input
          type="textarea"
          v-model="formData.jobDuty"
          placeholder="提示：不能少于50个字节（一个汉字计算两个字节）"
          rows="4"
        />
      </el-form-item>

      <!-- 任职要求 -->
      <el-form-item label="任职要求" prop="jobRequire">
        <el-input
          type="textarea"
          v-model="formData.jobRequire"
          placeholder="提示：不能少于50个字节（一个汉字计算两个字节）"
          rows="4"
          max-length="2000"
          show-word-limit
        />
      </el-form-item>

      <!-- 福利待遇 -->
      <el-form-item label="福利待遇" prop="welfare">
        <el-input
          type="textarea"
          v-model="formData.welfare"
          placeholder="提示：不能少于50个字节（一个汉字计算两个字节）"
          rows="4"
          max-length="1000"
          show-word-limit
        />
      </el-form-item>

      <!-- 职业发展 -->
      <el-form-item label="职业发展" prop="careerDev">
        <el-input
          type="textarea"
          v-model="formData.careerDev"
          placeholder="提示：不能少于50个字节（一个汉字计算两个字节）"
          rows="4"
          max-length="1000"
          show-word-limit
        />
      </el-form-item>

      <!-- 联系人 -->
      <el-form-item label="联系人" prop="contactPerson">
        <el-input
          v-model="formData.contactPerson"
          placeholder="请输入联系人姓名"
          max-length="20"
          show-word-limit
        />
      </el-form-item>

      <!-- 联系电话 -->
      <el-form-item label="联系电话" prop="contactPhone">
        <el-input
          v-model="formData.contactPhone"
          placeholder="请输入联系电话"
          max-length="11"
          show-word-limit
        />
      </el-form-item>

      <!-- 招聘截止日期 -->
      <el-form-item label="招聘截止日期" prop="deadline">
        <el-date-picker
          v-model="formData.deadline"
          type="date"
          placeholder="选择招聘截止日期"
          value-format="yyyy-MM-dd"
        />
      </el-form-item>

      <!-- 操作按钮 -->
      <el-form-item>
        <el-button type="default" @click="handleReturn">返回</el-button>
        <el-button type="primary" @click="handleSaveDraft">保存草稿</el-button>
        <el-button type="success" @click="handlePublish">发布</el-button>
        <el-button type="info" @click="handlePublishAndNext">发布并新增下一个</el-button>
      </el-form-item>
    </el-form>
  </el-dialog>
</template>

<script>
export default {
  name: 'RecruitForm',
  props: {
    addPostVisible: {
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
      isEdit: false, // 标记是否为编辑状态
      // 表单数据
      formData: {
        positionName: '',
        positionType: '',
        workCity: '',
        recruitMajor: '',
        educationReq: '',
        workType: '',
        recruitNum: 1,
        salaryRange: '',
        jobDuty: '',
        jobRequire: '',
        welfare: '',
        careerDev: '',
        contactPerson: '',
        contactPhone: '',
        deadline: ''
      },
      // 下拉选项模拟（实际可接口获取）
      positionTypeOptions: [],
      workCityOptions: [],
      recruitMajorOptions: [],
      educationReqOptions: [
        { label: '大专', value: 'juniorCollege' },
        { label: '本科', value: 'bachelor' },
        { label: '硕士', value: 'master' },
        { label: '博士', value: 'doctor' }
      ],
      workTypeOptions: [
        { label: '全职', value: 'fullTime' },
        { label: '兼职', value: 'partTime' }
      ],
      salaryRangeOptions: [
        { label: '5k-8k', value: '5k-8k' },
        { label: '8k-12k', value: '8k-12k' }
      ],
      // 表单校验规则
      rules: {
        positionName: [
          { required: true, message: '请输入职位名称', trigger: 'blur' }
        ],
        positionType: [
          { required: true, message: '请选择职位类别', trigger: 'change' }
        ],
        workCity: [
          { required: true, message: '请选择工作城市', trigger: 'change' }
        ],
        recruitMajor: [
          { required: true, message: '请选择招聘专业', trigger: 'change' }
        ],
        educationReq: [
          { required: true, message: '请选择学历要求', trigger: 'change' }
        ],
        workType: [
          { required: true, message: '请选择工作类型', trigger: 'change' }
        ],
        recruitNum: [
          { required: true, message: '请输入招聘人数', trigger: 'blur' }
        ],
        salaryRange: [
          { required: true, message: '请选择月薪范围', trigger: 'change' }
        ],
        jobDuty: [
          {
            validator: this.validateByteLength('jobDuty', 50),
            trigger: 'blur'
          }
        ],
        jobRequire: [
          {
            validator: this.validateByteLength('jobRequire', 50),
            trigger: 'blur'
          }
        ],
        welfare: [
          {
            validator: this.validateByteLength('welfare', 50),
            trigger: 'blur'
          }
        ],
        careerDev: [
          {
            validator: this.validateByteLength('careerDev', 50),
            trigger: 'blur'
          }
        ],
        contactPerson: [
          { required: true, message: '请输入联系人姓名', trigger: 'blur' }
        ],
        contactPhone: [
          {
            required: true,
            message: '请输入联系电话',
            trigger: 'blur'
          },
          {
            pattern: /^1\d{10}$/,
            message: '请输入正确的11位手机号码',
            trigger: 'blur'
          }
        ],
        deadline: [
          { required: true, message: '请选择招聘截止日期', trigger: 'change' }
        ]
      }
    };
  },

  watch: {
    // 监听父组件传递的 addPostVisible，同步到子组件内部的 dialogVisible
    addPostVisible(newVal) {
      this.dialogVisible = newVal;
      // 打开弹窗时重置表单校验
      if (newVal) {
        this.$nextTick(() => {
          this.$refs.recruitForm?.clearValidate();
        });
      }
    },
    // 监听 editData 变化，回显数据
    editData: {
      deep: true,
      handler(newVal) {
        if (newVal && Object.keys(newVal).length > 0) {
          this.isEdit = true;
          this.formData = { ...newVal };
        }
      }
    }
  },
  computed: {
    // 弹窗标题（根据表单状态变化）
    dialogTitle() {
      if (this.isEdit) return '编辑职位'
      return "新增职位"
    }
  },
  methods: {
    // 字节长度校验函数（通用）
    validateByteLength(field, minLength) {
      return (rule, value, callback) => {
        if (!value) {
          callback(new Error(`请输入内容`));
          return;
        }
        const byteLen = value.split('').reduce((total, char) => {
          return total + (char.match(/[^\x00-\xff]/) ? 2 : 1);
        }, 0);
        if (byteLen < minLength) {
          callback(new Error(`不能少于${minLength}个字节（一个汉字计算两个字节）`));
        } else {
          callback();
        }
      };
    },
    // 返回操作
    handleReturn() {
      //this.addPostVisible = false;
      this.$emit('dialog-closed');
      this.$message.info('已返回');
    },
    // 发布操作（子组件中）
    handlePublish() {
      this.$refs.recruitForm.validate((valid) => {
        if (valid) {
          // 1. 模拟接口请求（实际项目替换为真实接口）
          setTimeout(() => {
            // 2. 构造完整的新增数据（补充表格所需字段，如 updateTime、recruitStatus 等）
            const newPositionData = {
              ...this.formData, // 表单填写的核心数据
              id: Math.random().toString(36).substr(2, 9), // 临时生成唯一ID（实际由后端返回）
              updateTime: new Date().toLocaleString(), // 当前时间作为更新时间
              endDate: this.formData.deadline + ' 23:59:59', // 补充截止日期格式（匹配表格显示）
              clickCount: 0, // 初始点击次数为0
              recruitStatus: 'recruiting' // 发布后默认“招聘中”
            };

            // 3. 触发 add-success 事件，将新增数据传递给父组件
            this.$emit('add-success', newPositionData);

            this.$message.success('发布成功');
            this.addPostVisible = false; // 关闭弹窗
          }, 500);
        } else {
          Message.error('表单校验未通过，请完善信息');
        }
      });
    },
    // 保存草稿
    handleSaveDraft() {
      this.$refs.recruitForm.validate((valid) => {
        if (valid) {
          console.log('保存草稿数据：', this.formData);
          this.$emit('save-draft', this.formData);
          Message.success('草稿保存成功');
          this.addPostVisible = false;
        } else {
          Message.error('表单校验未通过，请完善信息');
        }
      });
    },
    // 发布并新增下一个
    handlePublishAndNext() {
      this.$refs.recruitForm.validate((valid) => {
        if (valid) {
          console.log('发布数据：', this.formData);
          this.$emit('publish-and-next', this.formData);
          Message.success('发布成功，准备新增下一个');
          // 重置表单
          this.formData = {
            positionName: '',
            positionType: '',
            workCity: '',
            recruitMajor: '',
            educationReq: '',
            workType: '',
            recruitNum: 1,
            salaryRange: '',
            jobDuty: '',
            jobRequire: '',
            welfare: '',
            careerDev: '',
            contactPerson: '',
            contactPhone: '',
            deadline: ''
          };
          this.$refs.recruitForm.resetFields();
        } else {
          Message.error('表单校验未通过，请完善信息');
        }
      });
    },
    // 招聘人数变化
    handleRecruitNumChange(val) {
      this.formData.recruitNum = val;
    },
    // 维护操作（示例，实际可跳转维护页面或调接口）
    handleMaintain(type) {
      Message.info(`点击维护${type}，可在此处理维护逻辑`);
    },
    // 弹窗关闭回调
    handleDialogClose() {
      this.$emit('dialog-closed');
      this.formData = {
        positionName: '',
        positionType: '',
        workCity: '',
        recruitMajor: '',
        educationReq: '',
        workType: '',
        recruitNum: 1,
        salaryRange: '',
        jobDuty: '',
        jobRequire: '',
        welfare: '',
        careerDev: '',
        contactPerson: '',
        contactPhone: '',
        deadline: ''
      };
      this.$refs.recruitForm.resetFields();
    }
  }
};
</script>

<style scoped>
.recruit-form {
  width: 100%;
}
.tip {
  font-size: 12px;
  color: #999;
  margin-top: 4px;
}
</style>
