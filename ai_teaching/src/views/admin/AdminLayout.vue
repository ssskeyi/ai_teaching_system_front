<template>
    <div class="admin-layout">
        <header class="app-header">
            <div class="logo">
                <h1>教学系统 - 管理员端</h1>
            </div>
            <div class="user-info">
                <span>{{ username }}</span>
                <button @click="logout" class="logout-btn">退出登录</button>
            </div>
        </header>

        <div class="layout-content">
            <aside class="sidebar">
                <nav>
                    <ul>
                        <li>
                            <router-link to="/admin/workbench" active-class="active">
                                备课工作台
                            </router-link>
                        </li>
                        <li>
                            <router-link to="/admin/resources" active-class="active">
                                资源展示
                            </router-link>
                            <ul class="sub-menu">
                                <li>
                                    <router-link to="/admin/resources/history" active-class="active">
                                        历史记录
                                    </router-link>
                                </li>
                                <li>
                                    <router-link to="/admin/resources/personal" active-class="active">
                                        个人资源
                                    </router-link>
                                </li>
                            </ul>
                        </li>
                        <li>
                            <router-link to="/admin/classes" active-class="active">
                                班级管理
                            </router-link>
                        </li>
                        <li class="admin-menu">
                            <router-link to="/admin/users" active-class="active">
                                用户管理
                            </router-link>
                        </li>
                        <li class="admin-menu">
                            <router-link to="/admin/resources-manage" active-class="active">
                                资源管理
                            </router-link>
                        </li>
                    </ul>
                </nav>
            </aside>

            <main class="main-content">
                <router-view></router-view>
            </main>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const username = ref('管理员用户');

onMounted(() => {
    // 在实际应用中，这里应该获取用户信息
    // 例如从localStorage或后端API获取
});

const logout = () => {
    // 清除登录状态
    localStorage.removeItem('isLoggedIn');
    localStorage.removeItem('userRole');

    // 跳转到登录页
    router.push('/login');
};
</script>

<style scoped>
.admin-layout {
    height: 100%;
    width: 100%;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
}

.app-header {
    background-color: #e74c3c;
    color: white;
    padding: 1rem 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
}

.logo h1 {
    margin: 0;
    font-size: 1.5rem;
}

.user-info {
    display: flex;
    align-items: center;
    gap: 1rem;
}

.logout-btn {
    background-color: transparent;
    color: white;
    border: 1px solid white;
    padding: 0.25rem 0.5rem;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s;
}

.logout-btn:hover {
    background-color: white;
    color: #e74c3c;
}

.layout-content {
    display: flex;
    flex: 1;
    height: calc(100% - 60px);
    /* 减去header高度 */
    width: 100%;
    overflow: hidden;
}

.sidebar {
    width: 250px;
    height: 100%;
    background-color: #f5f7fa;
    padding: 1rem 0;
    border-right: 1px solid #ddd;
    overflow-y: auto;
}

.sidebar nav ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.sidebar nav ul li {
    margin-bottom: 0.5rem;
}

.sidebar nav a {
    display: block;
    padding: 0.75rem 1.5rem;
    color: #2c3e50;
    text-decoration: none;
    transition: all 0.3s;
}

.sidebar nav a:hover,
.sidebar nav a.active {
    background-color: #e74c3c;
    color: white;
}

.admin-menu a {
    font-weight: bold;
}

.admin-menu a:hover,
.admin-menu a.active {
    background-color: #c0392b;
}

.sub-menu {
    padding-left: 1.5rem !important;
}

.sub-menu a {
    padding: 0.5rem 1.5rem !important;
    font-size: 0.9rem;
}

.main-content {
    flex: 1;
    height: 100%;
    width: 100%;
    padding: 1.5rem;
    overflow-y: auto;
}
</style>