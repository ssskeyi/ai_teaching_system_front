<template>
    <div class="class-detail">
        <h1>班级详情</h1>
        <p>这是班级详情页面的占位符，具体内容将根据设计图进行开发</p>
    </div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router';

const route = useRoute();
const classId = route.params.id;
</script>

<style scoped>
.class-detail {
    padding: 20px;
}
</style>