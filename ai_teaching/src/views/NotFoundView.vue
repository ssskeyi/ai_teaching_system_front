<template>
    <div class="not-found">
        <h1>404</h1>
        <p>页面未找到</p>
        <router-link to="/">返回首页</router-link>
    </div>
</template>

<script setup lang="ts">
// 404页面组件
</script>

<style scoped>
.not-found {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    text-align: center;
}

h1 {
    font-size: 6rem;
    margin-bottom: 1rem;
    color: #e74c3c;
}

p {
    font-size: 1.5rem;
    margin-bottom: 2rem;
    color: #333;
}

a {
    color: #3498db;
    text-decoration: none;
    font-size: 1.2rem;
    padding: 0.5rem 1rem;
    border: 1px solid #3498db;
    border-radius: 4px;
    transition: all 0.3s;
}

a:hover {
    background-color: #3498db;
    color: white;
}
</style>