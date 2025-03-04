<template>
    <div class="register-container">
        <div class="register-box">
            <h1>教学系统注册</h1>
            <form @submit.prevent="handleRegister">
                <div class="form-group">
                    <label for="username">用户名</label>
                    <input type="text" id="username" v-model="username" placeholder="请输入用户名" required />
                </div>
                <div class="form-group">
                    <label for="password">密码</label>
                    <input type="password" id="password" v-model="password" placeholder="请输入密码" required />
                </div>
                <div class="form-group">
                    <label for="confirmPassword">确认密码</label>
                    <input type="password" id="confirmPassword" v-model="confirmPassword" placeholder="请再次输入密码"
                        required />
                </div>
                <div class="form-group">
                    <label for="role">角色</label>
                    <select id="role" v-model="role" required>
                        <option value="">请选择角色</option>
                        <option value="teacher">教师</option>
                        <option value="student">学生</option>
                        <option value="admin">管理员</option>
                    </select>
                </div>
                <div class="form-actions">
                    <button type="submit" class="btn-register">注册</button>
                    <div class="login-link">
                        已有账号？<router-link to="/login">登录</router-link>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const username = ref('');
const password = ref('');
const confirmPassword = ref('');
const role = ref('');

const handleRegister = () => {
    // 表单验证
    if (password.value !== confirmPassword.value) {
        alert('两次输入的密码不一致');
        return;
    }

    // 在实际应用中，这里应该发送请求到后端注册用户
    console.log('注册信息：', {
        username: username.value,
        password: password.value,
        role: role.value
    });

    // 模拟注册成功，跳转到登录页
    alert('注册成功，请登录');
    router.push('/login');
};
</script>

<style scoped>
.register-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
    background-color: #f5f7fa;
}

.register-box {
    width: 400px;
    padding: 2rem;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
    text-align: center;
    color: #2c3e50;
    margin-bottom: 2rem;
}

.form-group {
    margin-bottom: 1.5rem;
}

label {
    display: block;
    margin-bottom: 0.5rem;
    color: #34495e;
    font-weight: 500;
}

input,
select {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 1rem;
}

input:focus,
select:focus {
    outline: none;
    border-color: #3498db;
    box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
}

.form-actions {
    margin-top: 2rem;
}

.btn-register {
    width: 100%;
    padding: 0.75rem;
    background-color: #2ecc71;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.3s;
}

.btn-register:hover {
    background-color: #27ae60;
}

.login-link {
    text-align: center;
    margin-top: 1rem;
    color: #7f8c8d;
}

.login-link a {
    color: #3498db;
    text-decoration: none;
}

.login-link a:hover {
    text-decoration: underline;
}
</style>