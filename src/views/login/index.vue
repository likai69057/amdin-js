<template>
  <div class="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li :class="{'current': tab.current}" v-for="(tab, index) in menuTab" :key="index" @click="toggleMenu(tab)">{{ tab.text }}</li>
      </ul>
      <!-- 登录表单 -->
      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="login-form demo-ruleForm" size="small">

        <el-form-item prop="pass" class="item-form">
          <label>邮箱</label>
          <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item prop="checkPass" class="item-form">
          <label>密码</label>
          <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item prop="age" class="item-form">
          <label>验证码</label>
          <el-row :gutter="11">
            <el-col :span="15">
              <el-input v-model.number="ruleForm.age"></el-input>
            </el-col>
            <el-col :span="9">
              <el-button class="block" type="success">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>

        <el-form-item>
          <el-button class="danger-btn block" type="danger" @click="submitForm('ruleForm')">提交</el-button>
        </el-form-item>
      </el-form>
      <!-- 登录表单 -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    var checkAge = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('年龄不能为空'))
      }
      setTimeout(() => {
        if (!Number.isInteger(value)) {
          callback(new Error('请输入数字值'))
        } else {
          if (value < 18) {
            callback(new Error('必须年满18岁'))
          } else {
            callback()
          }
        }
      }, 1000)
    }
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'))
      } else {
        if (this.ruleForm.checkPass !== '') {
          this.$refs.ruleForm.validateField('checkPass')
        }
        callback()
      }
    }
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'))
      } else if (value !== this.ruleForm.pass) {
        callback(new Error('两次输入密码不一致!'))
      } else {
        callback()
      }
    }
    return {
      menuTab: [
        { text: '登录', current: true },
        { text: '注册', current: false }
      ],
      isActive: true,
      ruleForm: {
        pass: '',
        checkPass: '',
        age: ''
      },
      rules: {
        pass: [
          { validator: validatePass, trigger: 'blur' }
        ],
        checkPass: [
          { validator: validatePass2, trigger: 'blur' }
        ],
        age: [
          { validator: checkAge, trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    toggleMenu (data) {
      this.menuTab.forEach(el => {
        el.current = false
      })
      data.current = true
    },
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style scoped lang="less">
.login{
  background-color: #344a5f;
  height: 100vh;
  /deep/.login-wrap{
    width: 330px;
    height: auto;
    margin: 0px auto;
    .menu-tab{
      text-align: center;
      li{
        display: inline-block;
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        color: #fff;
        border-radius: 2px;
        cursor: pointer;
      }
      // 选中时按钮的颜色
      .current{
        background-color: rgba(0, 0, 0, .1);
      }
    }
    .login-form{
      margin-top: 29px;
      label{
        display: block;
        font-size: 14px;
        color: #fff;
        margin-bottom: 3px;
      }
      .item-form{
        margin-bottom: 13px;
      }
      .danger-btn{
        margin-top: 19px;
      }
      .block{
        width: 100%;
      }
    }
  }
}
</style>
