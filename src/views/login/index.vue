<template>
   <div class="login-box">
      <img class="login-img" :src="loginIcon" />
      <div class="login-form">
         <h1>你好LYH</h1>
         <el-form
            ref="ruleFormRef"
            class="login-info"
            :model="userInfo"
            :rules="rules"
            show-message
            center
         >
            <el-form-item prop="username">
               <el-input
                  v-model="userInfo.username"
                  :prefix-icon="User"
                  placeholder="账号"
               />
            </el-form-item>
            <el-form-item prop="password">
               <el-input
                  v-model="userInfo.password"
                  :prefix-icon="Lock"
                  type="password"
                  placeholder="密码"
               />
            </el-form-item>
            <el-form-item>
               <el-button type="primary" style="width: 100%" @click="userLogin"
                  >登录</el-button
               >
            </el-form-item>
         </el-form>
      </div>
   </div>
</template>

<script lang="ts" setup>
import { User, Lock } from '@element-plus/icons-vue';
import { reactive } from 'vue';
import type { FormRules } from 'element-plus';
import { backToHome } from '@/utils/router';
import { useUserStoreHook } from '@/store/modules/user';
import loginIcon from '@/assets/imgs/login/team_up.svg';
import { debounce } from 'lodash';
console.log(2222);
const userStore = useUserStoreHook();

const userInfo = reactive({
   username: 'admin',
   password: 'admin',
});
const rules = reactive<FormRules>({
   username: [
      {
         required: true,
         message: '请输入用户名',
         trigger: 'blur',
      },
   ],
   password: [
      {
         required: true,
         message: '请输入密码',
         trigger: 'blur',
      },
   ],
});

const userLogin = debounce(() => {
   userStore.storeUserLogin(userInfo).then(() => {
      backToHome();
   });
}, 200);
</script>

<style lang="scss" scoped>
.login-box {
   display: flex;
   justify-content: space-evenly;
   align-items: center;
   height: 100vh;
   background: var(--sv3-backgroud-color);
   .login-form {
      width: 300px;
      display: flex;
      text-align: center;
      flex-direction: column;
      .login-info {
         height: max-content;
      }
   }
}

@media screen and (max-width: 900px) {
   .login-img {
      display: none;
   }
}
</style>
