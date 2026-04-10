<template>
  <div class="login-container">
    <div class="login-box">
      <div style="font-weight: bold; font-size: 30px; text-align: center; margin-bottom: 30px; color: #009372">欢 迎 注 册
      </div>
      <el-form :model="data.form" ref="formRef" :rules="data.rules">
        <el-form-item prop="username">
          <el-input :prefix-icon="User" size="large" v-model="data.form.username" placeholder="请输入账号"/>
        </el-form-item>
        <el-form-item prop="password">
          <el-input :prefix-icon="Lock" size="large" v-model="data.form.password" placeholder="请输入密码"
                    show-password/>
        </el-form-item>

        <el-form-item prop="newPassword">
          <el-input :prefix-icon="Lock" size="large" v-model="data.form.newPassword" placeholder="请确认密码"
                    show-password/>
        </el-form-item>

        <el-form-item>
          <el-button size="large" style="width: 100%; background-color:#009372; color: white;" @click="register">注 册</el-button>
        </el-form-item>
      </el-form>
      <div style="text-align: right;">
        已有账号？请 <a href="/login">登录</a>
      </div>
    </div>


    <div class="rotate">
      <section>
        <div><img src="../assets/imgs/派蒙.jpeg" title="1" alt="意义不明的图片"></div>
        <div><img src="../assets/imgs/派蒙.jpeg" title="2" alt="意义不明的图片"></div>
        <div><img src="../assets/imgs/派蒙.jpeg" title="3" alt="意义不明的图片"></div>
        <div><img src="../assets/imgs/派蒙.jpeg" title="4" alt="意义不明的图片"></div>
        <div><img src="../assets/imgs/派蒙.jpeg" title="5" alt="意义不明的图片"></div>
        <div><img src="../assets/imgs/派蒙.jpeg" title="6" alt="意义不明的图片"></div>
        <div><img src="../assets/imgs/派蒙.jpeg" title="7" alt="意义不明的图片"></div>
      </section>
    </div>

  </div>

</template>

<script setup>
import {reactive, ref} from "vue";
import {User, Lock} from "@element-plus/icons-vue";
import request from "@/utils/request";
import {ElMessage} from "element-plus";
import router from "@/router";

const data = reactive({
  form: {role: '普通用户'},
  rules: {
    username: [
      {required: true, message: '请输入账号', trigger: 'blur'},
    ],
    password: [
      {required: true, message: '请输入密码', trigger: 'blur'},
    ],
    newPassword: [
      {required: true, message: '请确认密码', trigger: 'blur'},
    ]
  }
})

const formRef = ref()

// 点击登录按钮的时候会触发这个方法
const register = () => {
  formRef.value.validate((valid => {
    if (valid) {
      // 调用后台的接口
      request.post('/register', data.form).then(res => {
        if (res.code === '200') {
          ElMessage.success("恭喜您！注册成功")
          router.push('/login')
        } else {
          ElMessage.error(res.msg)
        }
      })
    }
  })).catch(error => {
    console.error(error)
  })
}

</script>

<style scoped>

.login-container {
  background: #2e4339;
}


/* 3D旋转立方体 */
section {
  position: relative;
  width: 150px;
  height: 150px;
  margin: 0 auto;
  transform-style: preserve-3d;
  animation: rotate 18s linear infinite;
}

section:hover {
  animation-play-state: paused;
}

@keyframes rotate {
  0% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(-360deg);
  }
}

section div {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(255, 0, 0, 0.5);
}

section div img {
  width: 100%;
  height: 100%;
}

section div:nth-child(1) {
  transform: rotateY(0deg) translateZ(200px);
}

section div:nth-child(2) {
  transform: rotateY(60deg) translateZ(200px);
  box-shadow: 0 0 20px rgba(0, 255, 0, 0.5);
}

section div:nth-child(3) {
  transform: rotateY(120deg) translateZ(200px);
  box-shadow: 0 0 20px rgba(0, 0, 255, 0.5);
}

section div:nth-child(4) {
  transform: rotateY(180deg) translateZ(200px);
  box-shadow: 0 0 20px rgba(128, 0, 0, 0.5);

}

section div:nth-child(5) {
  transform: rotateY(240deg) translateZ(200px);
  box-shadow: 0 0 20px rgba(0, 128, 0, 0.5);
}

section div:nth-child(6) {
  transform: rotateY(300deg) translateZ(200px);
  box-shadow: 0 0 20px rgba(0, 0, 128, 0.5);
}


</style>