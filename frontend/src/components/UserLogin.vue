<template>
  <div class="userLogin">
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
      <button type="submit" @click.prevent="register">注册</button>
    </form>
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
  </div>
</template>

<script>
export default {
  name: 'UserLogin',
  data() {
    return {
      username: '',
      password: '',
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
      axios.post('/api/login', {
        username: this.loginUsername,
        password: this.loginPassword,
      })
          .then(response => {
// 登录成功
          })
          .catch(error => {
// 处理登录失败
          });
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
  background-color: #009688;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #008080;
}
</style>
