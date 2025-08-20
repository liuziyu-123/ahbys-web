<template>
  <div class="recruitment-form">
    <!-- 选择职位弹窗 -->
    <el-dialog
      title="选择招聘职位"
      :visible.sync="positionDialogVisible"
      width="50%"
      @close="cancel"
      destroy-on-close
    >
      <!-- 表单区域 -->
      <el-form
        ref="recruitmentForm"
        :model="form"
        :rules="rules"
        label-width="120px"
        size="medium"
      >
        <!-- 招聘学校 -->
        <el-form-item label="招聘学校" prop="recruitSchool">
          <el-select
            v-model="form.recruitSchool"
            placeholder="请选择招聘学校"
            clearable
            style="width: 300px"
          >
            <el-option
              v-for="item in schoolOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-form-item>

        <!-- 推荐渠道 -->
        <el-form-item label="推荐渠道" prop="recommendChannel">
          <el-select
            v-model="form.recommendChannel"
            placeholder="请选择推荐渠道"
            clearable
            style="width: 300px"
          >
            <el-option
              v-for="item in channelOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-form-item>

        <!-- 是否校友企业 -->
        <el-form-item label="是否校友企业" prop="isAlumniEnterprise">
          <el-radio-group v-model="form.isAlumniEnterprise">
            <el-radio :label="true">是</el-radio>
            <el-radio :label="false">否</el-radio>
          </el-radio-group>
        </el-form-item>

        <!-- 招聘类型 -->
        <el-form-item label="招聘类型" prop="recruitType">
          <el-select
            v-model="form.recruitType"
            placeholder="请选择招聘类型"
            clearable
            style="width: 300px"
          >
            <el-option
              v-for="item in typeOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-form-item>

        <!-- 是否推送学校简历 -->
        <el-form-item label="是否推送学校简历" prop="isPushResume">
          <el-radio-group v-model="form.isPushResume">
            <el-radio :label="true">是</el-radio>
            <el-radio :label="false">否</el-radio>
          </el-radio-group>
        </el-form-item>

        <!-- 招聘职位 -->
        <el-form-item label="招聘职位" prop="recruitPosition">
          <el-input
            v-model="form.recruitPosition"
            placeholder="请选择招聘职位"
            readonly
            style="width: 300px"
          />
          <el-button
            type="primary"
            style="margin-left: 10px"
            @click="openPositionDialog"
          >
            选择职位
          </el-button>
          <div class="tips">
            招聘职位选择请务必同步“招聘简章”中岗位数量、内容，不然导致无法线上宣传、曝光！
          </div>
        </el-form-item>

        <!-- 招聘简章 -->
        <el-form-item label="招聘简章" prop="recruitBrochure">
          <el-input
            v-model="form.recruitBrochure"
            placeholder="请选择招聘简章"
            readonly
            style="width: 300px"
          />
          <el-button
            type="primary"
            style="margin-left: 10px"
            @click="openBrochureDialog"
          >
            选择简章
          </el-button>
        </el-form-item>

        <!-- 申请说明 -->
        <el-form-item label="申请说明" prop="applyDesc">
          <el-input
            type="textarea"
            v-model="form.applyDesc"
            placeholder="请输入申请说明"
            rows="4"
            style="width: 500px"
          />
        </el-form-item>

        <!-- 提交按钮 -->
        <el-form-item>
          <el-button type="primary" @click="handleSubmit">提交</el-button>
          <el-button @click="handleReset" style="margin-left: 10px">返回</el-button>
        </el-form-item>
      </el-form>

    </el-dialog>

    <!-- 选择简章弹窗 -->
    <el-dialog
      title="选择招聘简章"
      :visible.sync="brochureDialogVisible"
      width="50%"
    >
      <el-table
        :data="brochureTableData"
        border
        style="width: 100%"
      >
        <el-table-column
          prop="name"
          label="简章名称"
          align="center"
        />
        <el-table-column
          prop="desc"
          label="简介"
          align="center"
        />
      </el-table>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancel">取消</el-button>
        <el-button type="primary" @click="confirmBrochure">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'RecruitmentForm',
  props: {
    visible: {
      type: Boolean,
      default: false
    },
  },
  data() {
    return {
      // 子组件内部“镜像变量”，用于绑定弹窗 visible，避免直接操作 props
      dialogVisible: false,
      // 表单数据
      form: {
        recruitSchool: '', // 招聘学校
        recommendChannel: '', // 推荐渠道
        isAlumniEnterprise: false, // 是否校友企业
        recruitType: '', // 招聘类型
        isPushResume: false, // 是否推送学校简历
        recruitPosition: '', // 招聘职位
        recruitBrochure: '', // 招聘简章
        applyDesc: '', // 申请说明
      },

      // 表单校验规则
      rules: {
        recruitSchool: [
          { required: true, message: '请选择招聘学校', trigger: 'change' },
        ],
        recommendChannel: [
          { required: true, message: '请选择推荐渠道', trigger: 'change' },
        ],
        isAlumniEnterprise: [
          { required: true, message: '请选择是否校友企业', trigger: 'change' },
        ],
        recruitType: [
          { required: true, message: '请选择招聘类型', trigger: 'change' },
        ],
        isPushResume: [
          { required: true, message: '请选择是否推送学校简历', trigger: 'change' },
        ],
        recruitPosition: [
          { required: true, message: '请选择招聘职位', trigger: 'change' },
        ],
        recruitBrochure: [
          { required: true, message: '请选择招聘简章', trigger: 'change' },
        ],
        applyDesc: [
          { required: true, message: '请输入申请说明', trigger: 'blur' },
        ],
      },

      // 下拉选项（模拟数据）
      schoolOptions: [
        { label: 'XX大学', value: 'university1' },
        { label: 'YY学院', value: 'college1' },
      ],
      channelOptions: [
        { label: '校园宣讲会', value: 'channel1' },
        { label: '招聘网站', value: 'channel2' },
      ],
      typeOptions: [
        { label: '校园招聘', value: 'campus' },
        { label: '社会招聘', value: 'social' },
      ],

      // 弹窗相关
      positionDialogVisible: false, // 职位弹窗显隐
      brochureDialogVisible: false, // 简章弹窗显隐
      positionTreeData: [
        {
          label: '技术岗位',
          children: [
            { label: '前端开发', value: 'frontEnd' },
            { label: '后端开发', value: 'backEnd' },
          ],
        },
        {
          label: '产品岗位',
          children: [
            { label: '产品经理', value: 'productManager' },
            { label: 'UI设计', value: 'uiDesign' },
          ],
        },
      ],
      treeProps: {
        children: 'children',
        label: 'label',
      },
      brochureTableData: [
        { name: '2025校园招聘简章', desc: '面向应届生' },
        { name: '2025社会招聘简章', desc: '面向社会人才' },
      ],
      selectedPosition: '', // 选中的职位
      selectedBrochure: '', // 选中的简章
    }
  },
  watch: {
    // 监听父组件传递的 addPostVisible，同步到子组件内部的 positionDialogVisible
    visible(newVal) {
      this.positionDialogVisible = newVal;
      // 打开弹窗时重置表单校验
      // if (newVal) {
      //   this.$nextTick(() => {
      //     this.$refs.form?.clearValidate();
      //   });
      // }
    },
  },
  methods: {
    // 提交表单
    handleSubmit() {
      this.$refs.recruitmentForm.validate((valid) => {
        if (valid) {
          // 校验通过，可调用接口提交数据
          this.$message.success('表单校验通过，准备提交！');
          // 示例：调用接口
          // this.$http.post('/api/submitRecruitment', this.form).then(res => { ... });
        } else {
          this.$message.error('表单校验未通过，请检查必填项！');
        }
      });
    },

    // 重置表单
    handleReset() {
     // this.$refs.recruitmentForm.resetFields();
      this.$emit('dialog-closed');


    },

    // 打开选择职位弹窗
    openPositionDialog() {
      this.positionDialogVisible = true;
    },

    // 打开选择简章弹窗
    openBrochureDialog() {
      this.brochureDialogVisible = true;
    },

    // 选择职位（树节点点击）
    handlePositionSelect(data) {
      this.selectedPosition = data.label;
    },

    // 确认选择职位
    confirmPosition() {
      if (this.selectedPosition) {
        this.form.recruitPosition = this.selectedPosition;
        this.positionDialogVisible = false;
      } else {
        this.$message.warning('请选择职位！');
      }
    },

    // 确认选择简章
    confirmBrochure() {
      if (this.selectedBrochure) {
        this.form.recruitBrochure = this.selectedBrochure;
        this.brochureDialogVisible = false;
      } else {
        // 模拟选择表格第一行
        this.form.recruitBrochure = this.brochureTableData[0].name;
        this.brochureDialogVisible = false;
      }
    },

    cancel(){
      this.$emit('dialog-closed');
    }
  },
}
</script>

<style scoped>
.recruitment-form {
  padding: 20px;
  background: #fff;
}

.tips {
  color: #ff5722;
  font-size: 12px;
  margin-top: 5px;
}

.dialog-footer {
  text-align: right;
}
</style>
