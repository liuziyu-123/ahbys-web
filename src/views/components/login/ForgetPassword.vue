<template>
    <div class="reset-password-page">
        <h2>重置密码</h2>
        <el-form :model="form" :rules="rules" ref="form" label-width="0px">
            <el-form-item prop="tenantName">
                <el-input v-model="tenantName" type="text" auto-complete="off" disabled placeholder="请输入租户名称">
                    <svg-icon slot="prefix" icon-class="house" class="el-input__icon input-icon" />
                </el-input>
            </el-form-item>

            <el-form-item prop="username">
                <el-input v-model="form.username" placeholder="请输入用户名" prefix-icon="el-icon-user" />
            </el-form-item>
            <el-form-item prop="phone">
                <el-input v-model="form.phone" placeholder="请输入手机号码" prefix-icon="el-icon-phone" />
            </el-form-item>
            <el-form-item prop="code">
                <el-input v-model="form.code" placeholder="请输入验证码" prefix-icon="el-icon-message">
                    <el-button slot="append" @click="getCode">获取验证码</el-button>
                </el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input v-model="form.password" placeholder="请输入密码" type="password" prefix-icon="el-icon-lock" />
            </el-form-item>
            <el-form-item prop="confirmPassword">
                <el-input v-model="form.confirmPassword" placeholder="确认密码" type="password"
                    prefix-icon="el-icon-lock" />
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="resetPassword">重置密码</el-button>
                <el-button @click="goBack">返回</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
    export default {
        name: 'ResetPassword',
        props: {
            tenantName: {
                type: String,
                required: false,
                default: ''
            }
        },


        data() {
            // 验证两次输入的密码是否一致
            const validateConfirmPassword = (rule, value, callback) => {
                if (value !== this.form.password) {
                    callback(new Error('两次输入的密码不一致'));
                } else {
                    callback();
                }
            };
            return {
                form: {
                    tenantName: '',
                    username: '',
                    phone: '',
                    code: '',
                    password: '',
                    confirmPassword: ''
                },
                // 表单验证规则
                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    phone: [
                        { required: true, message: '请输入手机号码', trigger: 'blur' },
                        { pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号码', trigger: 'blur' }
                    ],
                    code: [
                        { required: true, message: '请输入验证码', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' },
                        { min: 6, message: '密码长度不能小于6位', trigger: 'blur' }
                    ],
                    confirmPassword: [
                        { required: true, message: '请确认密码', trigger: 'blur' },
                        { validator: validateConfirmPassword, trigger: 'blur' }
                    ]
                },
                // 验证码倒计时
                countdown: 0,
                timer: null,
                showUstc: true
            };
        },


        watch: {
            tenantName: {
                handler(newVal, oldValue) {
                    console.log(newVal, oldValue,"forget");
                    this.tenantName =newVal;
                },
                immediate: true

            }
        },
        methods: {
            // 获取验证码
            getCode() {
                if (!this.form.phone) {
                    this.$message.error('请先输入手机号码');
                    return;
                }
                // 模拟发送验证码
                this.$message.success('验证码已发送');
                // 开始倒计时
                this.countdown = 60;
                this.timer = setInterval(() => {
                    if (this.countdown > 0) {
                        this.countdown--;
                    } else {
                        clearInterval(this.timer);
                    }
                }, 1000);
            },
            // 重置密码
            resetPassword() {
                this.$refs.form.validate((valid) => {
                    if (valid) {
                        // 模拟提交表单
                        this.$message.success('密码重置成功');
                        // 这里可以添加实际的重置密码逻辑
                    } else {
                        this.$message.error('请填写完整信息');
                    }
                });
            },
            // 返回
            goBack() {
                // 使用 $emit 触发自定义事件，并传递数据
                var isCompany=false
                if(this.tenantName =='COMPANY' ){
                    isCompany=true
                }else{
                    isCompany=false
                }
                this.$emit('forget-ustc', this.showUstc,isCompany);
            }
        },
        // 组件销毁时清除定时器
        beforeDestroy() {
            if (this.timer) {
                clearInterval(this.timer);
            }
        }
    };
</script>

<style scoped>
    .reset-password-page {
        max-width: 400px;
        margin: 50px auto;
        padding: 20px;
        border: 1px solid #ebeef5;
        border-radius: 8px;
        box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    }

    .reset-password-page h2 {
        text-align: center;
        margin-bottom: 30px;
    }

    .el-form-item {
        margin-bottom: 20px;
    }
</style>