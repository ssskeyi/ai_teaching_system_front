<template>
    <div class="history-view">
        <!-- 顶部搜索和操作区 -->
        <div class="top-actions">
            <div class="search-bar">
                <input type="text" v-model="searchQuery" placeholder="单击输入关键词或标题" @input="handleSearch" />
                <button class="search-btn">搜索</button>
            </div>
            <div class="action-buttons">
                <button class="batch-btn">批量操作</button>
                <button class="export-btn">导出记录</button>
            </div>
        </div>

        <!-- 资源列表 -->
        <div class="resources-list">
            <!-- 教案 -->
            <div class="resource-section">
                <h3>教案</h3>
                <div class="resource-items">
                    <div class="resource-item" v-for="(item, index) in lessonPlans" :key="'lesson-' + index">
                        <div class="title">{{ item.title }}</div>
                        <div class="date">{{ item.date }}</div>
                        <button class="edit-btn">编辑</button>
                    </div>
                </div>
            </div>

            <!-- 课件 -->
            <div class="resource-section">
                <h3>课件</h3>
                <div class="resource-items">
                    <div class="resource-item" v-for="(item, index) in courseware" :key="'courseware-' + index">
                        <div class="title">{{ item.title }}</div>
                        <div class="date">{{ item.date }}</div>
                        <button class="edit-btn">编辑</button>
                    </div>
                </div>
            </div>

            <!-- 视频 -->
            <div class="resource-section">
                <h3>视频</h3>
                <div class="resource-items">
                    <div class="resource-item" v-for="(item, index) in videos" :key="'video-' + index">
                        <div class="title">{{ item.title }}</div>
                        <div class="date">{{ item.date }}</div>
                        <button class="edit-btn">编辑</button>
                    </div>
                </div>
            </div>

            <!-- 练习题 -->
            <div class="resource-section">
                <h3>练习题</h3>
                <div class="resource-items">
                    <div class="resource-item" v-for="(item, index) in exercises" :key="'exercise-' + index">
                        <div class="title">{{ item.title }}</div>
                        <div class="date">{{ item.date }}</div>
                        <button class="edit-btn">编辑</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const searchQuery = ref('');

// 模拟数据
const lessonPlans = ref([
    { title: '2024年高等数学上期第一课', date: '2024/9/13' },
    { title: '2024年高等数学上期第二课', date: '2024/4/15' }
]);

const courseware = ref([
    { title: '名称', date: '生成日期' },
    { title: '名称', date: '生成日期' }
]);

const videos = ref([
    { title: '名称', date: '生成日期' },
    { title: '名称', date: '生成日期' }
]);

const exercises = ref([
    { title: '2024年高等数学上期第一期中培考题（一）', date: '2024/2/27' },
    { title: '名称', date: '生成日期' }
]);

const handleSearch = () => {
    console.log('搜索:', searchQuery.value);
    // 实现搜索逻辑
};
</script>

<style scoped>
.history-view {
    padding: 20px;
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.top-actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
}

.search-bar {
    display: flex;
    gap: 10px;
    flex: 1;
    max-width: 500px;
}

.search-bar input {
    flex: 1;
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 14px;
}

.search-btn {
    padding: 8px 20px;
    background-color: #666;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.action-buttons {
    display: flex;
    gap: 10px;
}

.batch-btn,
.export-btn {
    padding: 8px 20px;
    background-color: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.resources-list {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 20px;
    overflow-y: auto;
}

.resource-section {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.resource-section h3 {
    margin: 0 0 16px 0;
    font-size: 16px;
    color: #333;
}

.resource-items {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.resource-item {
    display: flex;
    align-items: center;
    padding: 12px;
    background: #f5f7fa;
    border-radius: 4px;
}

.resource-item .title {
    flex: 1;
    font-size: 14px;
    color: #333;
}

.resource-item .date {
    color: #666;
    font-size: 14px;
    margin: 0 20px;
}

.edit-btn {
    padding: 6px 16px;
    background-color: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.edit-btn:hover {
    background-color: #3a70e3;
}
</style>