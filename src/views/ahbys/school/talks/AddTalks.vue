<template>
  <el-dialog
    title="宣讲信息填写"
    :visible.sync="dialogVisible"
    width="60%"
    @close="handleClose"
  >
    <el-form :model="form" label-width="100px" ref="form">
      <el-form-item label="宣讲学校" prop="school">
        <el-select v-model="form.school" placeholder="请选择学校">
          <el-option
            v-for="item in schoolOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
        <span style="margin-left: 10px;">没有学校可选？<a href="javascript:;" @click="handleJoinSchool">点击入驻学校</a></span>
      </el-form-item>
      <el-form-item label="宣讲方式" prop="way">
        <el-select v-model="form.way" placeholder="请选择宣讲方式">
          <el-option
            v-for="item in wayOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="日期及场次" prop="dateSession">
        <el-input v-model="form.dateSession" placeholder="点击选择日期及场次"></el-input>
      </el-form-item>
      <el-form-item label="宣讲主题" prop="theme">
        <el-input v-model="form.theme" placeholder="请输入宣讲主题"></el-input>
      </el-form-item>
      <el-form-item label="联系手机" prop="phone">
        <el-input v-model="form.phone" placeholder="请输入联系手机"></el-input>
      </el-form-item>
      <el-form-item label="单位联系人" prop="contact">
        <el-input v-model="form.contact" placeholder="请输入单位联系人"></el-input>
      </el-form-item>
      <el-form-item label="单位座机" prop="tel">
        <el-input v-model="form.tel" placeholder="请输入单位座机"></el-input>
      </el-form-item>
      <el-form-item label="薪资水平" prop="salary">
        <el-select v-model="form.salary" placeholder="请选择薪资水平">
          <el-option
            v-for="item in salaryOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="其他要求" prop="otherReq">
        <el-input
          type="textarea"
          v-model="form.otherReq"
          placeholder="请输入其他要求"
        ></el-input>
      </el-form-item>
      <el-form-item label="招聘简章" prop="recruitment">
        <el-input v-model="form.recruitment" placeholder="点击选择招聘简章"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">返回</el-button>
      <el-button type="primary" @click="handleConfirm">确定</el-button>
    </div>
  </el-dialog>
</template>

<script>

export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    },
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
      dialogVisible: false, // 控制弹窗显示隐藏

      form: {
        school: '',
        way: '',
        dateSession: '',
        theme: '',
        phone: '',
        contact: '',
        tel: '',
        salary: '',
        otherReq: '',
        recruitment: ''
      },
      schoolOptions: [
        // 这里可根据实际接口或数据填充，示例选项
        { label: '学校A', value: 'schoolA' },
        { label: '学校B', value: 'schoolB' }
      ],
      wayOptions: [
        { label: '线上宣讲', value: 'online' },
        { label: '线下宣讲', value: 'offline' }
      ],
      salaryOptions: [
        { label: '5k-8k', value: '5-8k' },
        { label: '8k-12k', value: '8-12k' }
      ]
    };
  },
  watch: {
    // 监听弹窗显隐，初始化/销毁富文本编辑器
    visible(val) {
      // if (val) {
      //   this.initEditor()
      // } else {
      //   this.editor && this.editor.destroy()
      // }
      this.dialogVisible = val;
    }
  },
  methods: {
    handleClose() {
      // 弹窗关闭时的回调，可做重置表单等操作
      //this.$refs.form.resetFields();
      this.$emit('update:visible', false);
    },
    handleJoinSchool() {
      // 点击入驻学校的逻辑，比如跳转到入驻页面等
      console.log('执行入驻学校逻辑');
    },
    handleConfirm() {
      // 点击确定的逻辑，比如提交表单数据到后端
      this.$refs.form.validate((valid) => {
        if (valid) {
          // 假设通过 axios 提交数据，这里需要先引入 axios 并配置
          // import axios from 'axios';
          // axios.post('/your-api-url', this.form).then(res => {
          //   console.log('提交成功', res);
          //   this.dialogVisible = false;
          // }).catch(err => {
          //   console.error('提交失败', err);
          // });
          console.log('表单校验通过，可提交数据：', this.form);
          this.dialogVisible = false;
        } else {
          console.log('表单校验不通过');
        }
      });
    },
  }
};
</script>

<style scoped>
.dialog-footer {
  text-align: right;
}
</style>
