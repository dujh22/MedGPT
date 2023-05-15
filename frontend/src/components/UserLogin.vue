<template>
  <div class="userLogin">
    <el-tabs v-model="activeTab">
      <el-tab-pane label="登录" name="login">
        <h2>登录</h2>
        <form>
          <div class="form-group">
            <label for="login-username">用户名</label>
            <input type="text" id="login-username" v-model="loginUsername" required>
          </div>
          <div class="form-group">
            <label for="login-password">密码</label>
            <input type="password" id="login-password" v-model="loginPassword" required>
          </div>
          <button type="submit" @click.prevent="login">登录</button>
        </form>
      </el-tab-pane>
      <el-tab-pane label="注册" name="register">
        <h2>注册</h2>
        <form>
          <div class="form-group">
            <label for="username">用户名</label>
            <input type="text" id="username" v-model="username" required>
          </div>
          <div class="form-group">
            <label for="password">密码</label>
            <input type="password" id="password" v-model="password" required>
          </div>
          <div class="form-group">
            <label for="password-ensure">确认密码</label>
            <input type="password" id="password-ensure" v-model="passwordEnsure" required>
          </div>
          <button type="submit" @click.prevent="register">注册</button>
        </form>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'UserLogin',
  data() {
    return {
      activeTab: "login",
      username: '',
      password: '',
      passwordEnsure: '',
      loginUsername: '',
      loginPassword: '',
    };
  },
  methods: {
    register() {
      if (this.username === '') {
        alert('请输入用户名');
        return;
      }
      if (this.password === '') {
        alert('请输入密码');
        return;
      }
      if (this.password !== this.passwordEnsure) {
        alert('密码不一致');
        return;
      }
      axios.post('/api/register', {
        username: this.username,
        password: this.password,
      })
          .then(response => {
          })
          .catch(error => {
          });
    },
    login() {
      if (this.loginUsername === '') {
        alert('请输入用户名');
        return;
      }
      if (this.loginPassword === '') {
        alert('请输入密码');
        return;
      }
      this.$emit('login-success', {
        'username': this.loginUsername,
        'token': 'fake-token'
      })
      // axios.post('/api/login', {
      //   username: this.loginUsername,
      //   password: this.loginPassword,
      // })
      //     .then(response => {
      //       this.$emit('login-success', response['data']['token'])
      //     })
      //     .catch(error => {
      //       alert('登录失败');
      //     });
    },
  },
};
</script>

<style>
.card {
  width: 400px;
  border: 1px solid #ccc;
  padding: 20px;
  margin: 20px;
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 5px 10px;
  background-color: #422C8E;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #422C8E;
}
</style>
