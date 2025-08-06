<template>
    <div class="register-container">
        <h2>单位注册</h2>
        <el-form ref="registerForm" :model="registerForm" :rules="registerRules" label-width="140px">
            <span style="font-size: 18px; color: rgb(14, 144, 210);">注册信息</span>
            <div class="el-divider" style="--el-border-style: solid; margin: 15px 0px;"></div>
            <el-form-item label="统一社会信用代码" prop="socialCreditCode">
                <el-input v-model="registerForm.socialCreditCode" placeholder="请输入统一社会信用代码" />
            </el-form-item>
            <el-form-item label="注册手机号" prop="phone">
                <el-input v-model="registerForm.phone" placeholder="请输入注册手机号" />
            </el-form-item>
            <el-form-item label="短信验证码" prop="verificationCode">
                <el-input v-model="registerForm.verificationCode" placeholder="请输入验证码" style="width: 60%" />
                <el-button type="primary" @click="getVerificationCode" :disabled="isButtonDisabled">
                    {{ buttonText }}
                </el-button>
            </el-form-item>
            <el-form-item label="初始密码" prop="password">
                <el-input v-model="registerForm.password" placeholder="请输入初始密码" type="password" />
            </el-form-item>
            <el-form-item label="确认密码" prop="confirmPassword">
                <el-input v-model="registerForm.confirmPassword" placeholder="请输入确认密码" type="password" />
            </el-form-item>
            <span style="font-size: 18px; color: rgb(14, 144, 210);">单位信息</span>
            <div class="el-divider" style="--el-border-style: solid; margin: 15px 0px;"></div>
            <el-form-item label="单位名称" prop="unitName">
                <el-input v-model="registerForm.unitName" placeholder="请输入单位名称" />
            </el-form-item>
            <el-form-item label="单位性质" prop="unitNature">
                <el-select v-model="registerForm.unitNature" placeholder="请选择单位性质" class="select-width">
                    <el-option label="企业" value="企业" />
                    <el-option label="事业单位" value="事业单位" />
                    <el-option label="社会团体" value="社会团体" />
                </el-select>
            </el-form-item>
            <el-form-item label="单位行业" prop="unitIndustry">
                <el-select v-model="registerForm.unitIndustry" placeholder="请选择单位行业" class="select-width">
                    <el-option label="制造业" value="制造业" />
                    <el-option label="服务业" value="服务业" />
                    <el-option label="教育业" value="教育业" />
                </el-select>
            </el-form-item>
            <el-form-item label="企业类型" prop="enterpriseType">
                <el-select v-model="registerForm.enterpriseType" placeholder="请选择企业类型" class="select-width">
                    <el-option label="国有企业" value="国有企业" />
                    <el-option label="民营企业" value="民营企业" />
                    <el-option label="外资企业" value="外资企业" />
                </el-select>
            </el-form-item>
            <el-form-item label="经济类型" prop="economicType">
                <el-select v-model="registerForm.economicType" placeholder="请选择经济类型" class="select-width">
                    <el-option label="国有经济" value="国有经济" />
                    <el-option label="集体经济" value="集体经济" />
                    <el-option label="私营经济" value="私营经济" />
                </el-select>
            </el-form-item>
            <el-form-item label="人员规模" prop="personnelScale">
                <el-select v-model="registerForm.personnelScale" placeholder="请选择人员规模" class="select-width">
                    <el-option label="1-50人" value="1-50人" />
                    <el-option label="51-100人" value="51-100人" />
                    <el-option label="100人以上" value="100人以上" />
                </el-select>
            </el-form-item>
            <el-form-item label="网申地址" prop="applicationAddress">
                <el-input v-model="registerForm.applicationAddress" placeholder="请输入网申地址" />
            </el-form-item>

            <span style="font-size: 18px; color: rgb(14, 144, 210);">联系方式</span>
            <div class="el-divider" style="--el-border-style: solid; margin: 15px 0px;"></div>
            <el-form-item label="联系人" prop="contactPerson">
                <el-input v-model="registerForm.contactPerson" placeholder="请输入联系人" />
            </el-form-item>
            <el-form-item label="联系电话" prop="contactPhone">
                <el-input v-model="registerForm.contactPhone" placeholder="请输入联系电话" />
            </el-form-item>
            <el-form-item label="单位邮箱" prop="unitEmail">
                <el-input v-model="registerForm.unitEmail" placeholder="请输入单位邮箱" />
            </el-form-item>
            <el-form-item label="单位地址" prop="unitAddress">
                <el-input v-model="registerForm.unitAddress" placeholder="请输入单位地址" />
            </el-form-item>
            <el-form-item label="邮政编码" prop="postalCode">
                <el-input v-model="registerForm.postalCode" placeholder="请输入邮政编码" />
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForms" style="width: 100%;">
                    注册
                </el-button>
            </el-form-item>
            <el-form-item>
                <el-button plain @click="goToUstc" style="width: 100%;">
                    已有账号？去登录
                </el-button>
            </el-form-item>
            <!--    
        <el-form-item>
          <el-button type="primary" @click="submitForm('registerForm')">
            注册
          </el-button>
        </el-form-item> -->
        </el-form>
    </div>
</template>

<script>
    export default {
        name: 'UnitRegister',
        data() {
            // 验证联系电话的规则
            const contactPhone = (rule, value, callback) => {
                const phoneReg = /^1[3-9]\d{9}$/;
                if (!value) {
                    callback(new Error('请输入联系电话'));
                } else if (!phoneReg.test(value)) {
                    callback(new Error('请输入正确的联系电话'));
                } else {
                    callback();
                }
            };
            // 验证手机号的规则
            const validatePhone = (rule, value, callback) => {
                const phoneReg = /^1[3-9]\d{9}$/;
                if (!value) {
                    callback(new Error('请输入注册手机号'));
                } else if (!phoneReg.test(value)) {
                    callback(new Error('请输入正确的手机号'));
                } else {
                    callback();
                }
            };

            // 验证邮箱的规则
            const validateEmail = (rule, value, callback) => {
                const emailReg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
                if (!value) {
                    callback(new Error('请输入单位邮箱'));
                } else if (!emailReg.test(value)) {
                    callback(new Error('请输入正确的邮箱'));
                } else {
                    callback();
                }
            }
            // 验证两次输入的密码是否一致
            const validateConfirmPassword = (rule, value, callback) => {
                if (value !== this.registerForm.password) {
                    callback(new Error('两次输入的密码不一致'));
                } else {
                    callback();
                }

            };
            return {
                registerForm: {
                    socialCreditCode: '',
                    phone: '',
                    verificationCode: '',
                    password: '',
                    confirmPassword: '',
                    unitName: '',
                    unitNature: '',
                    unitIndustry: '',
                    enterpriseType: '',
                    economicType: '',
                    personnelScale: '',
                    enterpriseType: '',
                    economicType: '',
                    personnelScale: '',
                    applicationAddress: '',
                    applicationAddress: '',
                    contactPerson: '',
                    contactPhone: '',
                    unitEmail: '',
                    unitAddress: '',
                    postalCode: '',
                },
                registerRules: {
                    //注册信息
                    socialCreditCode: [
                        { required: true, message: '请输入统一社会信用代码', trigger: 'blur' },
                    ],
                    phone: [
                        { required: true, validator: validatePhone, trigger: 'blur' },
                    ],
                    verificationCode: [
                        { required: true, message: '请输入验证码', trigger: 'blur' },
                    ],
                    password: [
                        { required: true, message: '请输入初始密码', trigger: 'blur' },
                    ],
                    confirmPassword: [
                        { required: true, message: '请输入确认密码', trigger: 'blur' },
                        { validator: validateConfirmPassword, trigger: 'blur' },
                    ],

                    //单位信息
                    unitName: [
                        { required: true, message: '请输入单位名称', trigger: 'blur' },
                    ],
                    unitNature: [
                        { required: true, message: '请选择单位性质', trigger: 'change' },
                    ],
                    unitIndustry: [
                        { required: true, message: '请选择单位行业', trigger: 'change' },
                    ],
                    // enterpriseType: [
                    //     { required: true, message: '请选择企业类型', trigger: 'change' },
                    // ],
                    // economicType: [
                    //     { required: true, message: '请选择经济类型', trigger: 'change' },
                    // ],
                    // personnelScale: [
                    //     { required: true, message: '请选择人员规模', trigger: 'change' },
                    // ],
                    // applicationAddress: [
                    //     { required: true, message: '请选择网申地址', trigger: 'change' },
                    // ],

                    //联系方式
                    contactPerson: [
                        { required: true, message: '请输入联系人', trigger: 'blur' },
                    ],
                    contactPhone: [
                        { required: true, validator: contactPhone, trigger: 'blur' },
                    ],
                    unitEmail: [
                        { required: true, validator: validateEmail, trigger: 'blur' },
                    ],
                    unitAddress: [
                        { required: true, message: '请输入单位地址', trigger: 'blur' },
                    ],




                },
                countdown: 60,
                isButtonDisabled: false,
                buttonText: '获取验证码',
                //显示Ustc
                showUstc:true
            };

        },
        methods: {
            // 获取验证码的方法
            getVerificationCode() {
                // 先对手机号进行验证
                this.$refs.registerForm.validateField('phone', (error) => {
                    if (!error) {
                        this.isButtonDisabled = true;
                        // 模拟倒计时
                        const timer = setInterval(() => {
                            this.countdown--;
                            this.buttonText = `${this.countdown}秒后重新获取`;
                            if (this.countdown <= 0) {
                                clearInterval(timer);
                                this.countdown = 60;
                                this.buttonText = '获取验证码';
                                this.isButtonDisabled = false;
                            }
                        }, 1000);
                        // 这里可以调用后端接口发送验证码
                        console.log('向手机号', this.registerForm.phone, '发送验证码');
                    }
                });
            },
            // 提交表单的方法
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        // 表单验证通过，调用后端接口进行注册
                        console.log('表单验证通过，准备提交注册信息', this.registerForm);
                        // 这里可以调用后端注册接口
                    } else {
                        console.log('表单验证失败');
                        return false;
                    }
                });
            },

            //将showChild 传入父级
            goToUstc(){
                // 使用 $emit 触发自定义事件，并传递数据
                this.$emit('company-ustc', this.showUstc);
            }


        },
    };
</script>

<style>
    h2 {
        font-size: 1.875rem;
        line-height: 2.25rem;
        color: black;
        text-align: center;
    }

    .el-divider {
        border-top: 1px var(--el-border-color) var(--el-border-style);
        display: block;
        height: 1px;
        margin: 24px 0;
        width: 100%;
        position: relative;
    }

    .register-container {

        max-height: 750px;
        /* 设置最大高度，超出部分显示滚动条 */
    }

    .select-width {
        width: 100%;
    }
</style>