<template>
  <div class="login-container">
    <h2>ClawCode 登录</h2>
    <form @submit.prevent="handleLogin">
      <div class="form-group">
        <label for="username">用户名:</label>
        <input 
          type="text" 
          id="username" 
          v-model="username" 
          required 
        />
      </div>
      <div class="form-group">
        <label for="password">密码:</label>
        <input 
          type="password" 
          id="password" 
          v-model="password" 
          required 
        />
      </div>
      <button type="submit">登录</button>
      <p v-if="error" class="error">{{ error }}</p>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Login',
  data() {
    return {
      username: '',
      password: '',
      error: ''
    };
  },
  methods: {
    async handleLogin() {
      try {
        // 调用后端验证API (使用内网IP)
        const response = await axios.post('http://172.24.43.231:8090/api/auth/login', {
          username: this.username,
          password: this.password
        });
        
        if (response.data.success) {
          // 登录成功，跳转到主页面
          this.$router.push('/dashboard');
        } else {
          this.error = '用户名或密码错误';
        }
      } catch (err) {
        console.error('登录失败:', err);
        this.error = '服务器连接失败，请稍后重试';
      }
    }
  }
};
</script>

<style scoped>
.login-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.login-container h2 {
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.error {
  color: red;
  margin-top: 10px;
  text-align: center;
}
</style>