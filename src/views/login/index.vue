<template>
  <div class="login-container">
    <el-form ref="formRef" :model="form" class="login-form" :rules="rules">
      <div class="title-container">
        <h3 class="title">用户登录</h3>
      </div>
      <el-form-item prop="name">
        <el-icon :size="20" class="svg-container">
          <user />
        </el-icon>
        <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-icon :size="20" class="svg-container">
          <lock />
        </el-icon>
        <el-input v-model="form.password"></el-input>
      </el-form-item>
      <el-button type="primary" class="login-button" @click="handleLogin">登录</el-button>
    </el-form>
  </div>
</template>

<script setup>
  import {reactive, ref} from 'vue'
  import { Edit, User, Lock } from '@element-plus/icons-vue'
  import {useStore} from 'vuex'

  const store = useStore()
  const formRef = ref(null);
  const form = ref({
    username: "admin",
    password: "123456"
  })

  const rules = reactive({
    username: [
      {
        required: true,
        message: '请输入用户名',
        trigger: 'blur',
      },
      {
        min: 3,
        max: 5,
        message: 'Length should be 3 to 5',
        trigger: 'blur',
      }
    ],
    password: [
      {
        required: true,
        message: '请输入密码',
        trigger: 'blur',
      }
    ]
  })
  const handleLogin = () => {
    formRef.value.validate(async (valid) => {
      if (valid) {
        store.dispatch('app/login', form.value)
      } else {
        console.log('error submit:登录验证不通过!')
        return false
      }
    })
  }
</script>

<style lang="scss" scoped>
  $bg: #2d3a4b;
  $dark-gray: #889aa4;
  $light-gray: #eee;
  $cursor: #fff;

  .login-container {
    min-height: 100%;
    width: 100%;
    background-color: $bg;
    overflow: hidden;

    .login-form {
      position: relative;
      width: 520px;
      max-width: 100%;
      padding: 160px 35px 0;
      margin: 0 auto;
      overflow: hidden;

      ::v-deep .el-form-item {
        border: 1px solid rgba(255, 255, 255, 0.1);
        background: rgba(0, 0, 0, 0.1);
        border-radius: 5px;
        color: #454545;
      }

      ::v-deep .el-input {
        display: inline-block;
        height: 47px;
        width: 85%;

        input {
          background: transparent;
          border: 0px;
          -webkit-appearance: none;
          border-radius: 0px;
          padding: 12px 5px 12px 15px;
          color: $light-gray;
          height: 47px;
          caret-color: $cursor;
        }
      }

      .login-button {
        width: 100%;
        box-sizing: border-box;
      }
    }

    .tips {
      font-size: 16px;
      line-height: 28px;
      color: #fff;
      margin-bottom: 10px;

      span {
        &:first-of-type {
          margin-right: 16px;
        }
      }
    }

    .svg-container {
      padding: 6px 5px 6px 15px;
      color: $dark-gray;
      vertical-align: middle;
      display: inline-block;
    }

    .title-container {
      position: relative;

      .title {
        font-size: 26px;
        color: $light-gray;
        margin: 0px auto 40px auto;
        text-align: center;
        font-weight: bold;
      }

      ::v-deep .lang-select {
        position: absolute;
        top: 4px;
        right: 0;
        background-color: white;
        font-size: 22px;
        padding: 4px;
        border-radius: 4px;
        cursor: pointer;
      }
    }

    .show-pwd {
      font-size: 16px;
      color: $dark-gray;
      cursor: pointer;
      user-select: none;
    }
  }
</style>