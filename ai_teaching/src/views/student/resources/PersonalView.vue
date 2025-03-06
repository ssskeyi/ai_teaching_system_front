<template>
    <div class="personal-view">
        <!-- 顶部操作区 -->
        <div class="top-actions">
            <div class="search-bar">
                <input type="text" v-model="searchQuery" placeholder="单击输入关键词或标题" @input="handleSearch" />
                <button class="search-btn">搜索</button>
            </div>
            <button class="upload-btn" @click="handleUpload">+ 上传资源</button>
        </div>

        <!-- 资源类型标签 -->
        <div class="resource-types">
            <button v-for="type in resourceTypes" :key="type.value"
                :class="['type-btn', { active: type.value === activeType }]" @click="activeType = type.value">
                {{ type.label }}
            </button>
        </div>

        <!-- 资源列表 -->
        <div class="resource-list">
            <table>
                <thead>
                    <tr>
                        <th>名称</th>
                        <th>生成时间</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in resources" :key="index">
                        <td class="resource-type">
                            <span class="type-tag" :class="item.type">{{ item.type }}</span>
                            {{ item.name }}
                        </td>
                        <td>{{ item.date }}</td>
                        <td class="actions">
                            <button class="action-btn edit">编辑</button>
                            <button class="action-btn delete">删除</button>
                        </td>
                    </tr>
                </tbody>
            </table>

            <!-- 查看更多按钮 -->
            <div class="load-more">
                <button class="more-btn" @click="loadMore">查看更多</button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const searchQuery = ref('');
const activeType = ref('all');

// 资源类型
const resourceTypes = [
    { label: '教材', value: 'textbook' },
    { label: '教案', value: 'lesson' },
    { label: '习题', value: 'exercise' },
    { label: '图片', value: 'image' },
    { label: '视频', value: 'video' },
    { label: '课件', value: 'courseware' }
];

// 模拟资源数据
const resources = ref([
    { name: '2024年高等数学上期中模考题（三）', type: '习题', date: '2024/5/27' },
    { name: '关于......的文献研究', type: '教材', date: '2024/3/27' },
    { name: '大物练习题', type: '习题', date: '2024/1/2' },
    { name: '练习题三', type: '习题', date: '2023/9/27' },
    { name: '高等数学第三章PPT', type: '课件', date: '2023/9/10' },
    { name: '古诗词鉴赏专题', type: '教案', date: '2023/5/10' }
]);

// 处理搜索
const handleSearch = () => {
    console.log('搜索:', searchQuery.value);
    // 实现搜索逻辑
};

// 处理上传
const handleUpload = () => {
    console.log('上传资源');
    // 实现上传逻辑
};

// 加载更多
const loadMore = () => {
    console.log('加载更多');
    // 实现加载更多逻辑
};
</script>

<style scoped>
.personal-view {
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

.upload-btn {
    padding: 8px 20px;
    background-color: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
}

.resource-types {
    display: flex;
    gap: 10px;
    padding: 10px 0;
}

.type-btn {
    padding: 6px 16px;
    border: none;
    border-radius: 4px;
    background-color: #f5f7fa;
    color: #666;
    cursor: pointer;
    transition: all 0.3s;
}

.type-btn.active {
    background-color: #4c84ff;
    color: white;
}

.resource-list {
    flex: 1;
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    overflow-y: auto;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th {
    text-align: left;
    padding: 12px;
    color: #666;
    font-weight: normal;
    border-bottom: 1px solid #eee;
}

td {
    padding: 12px;
    border-bottom: 1px solid #eee;
}

.resource-type {
    display: flex;
    align-items: center;
    gap: 8px;
}

.type-tag {
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 12px;
    color: white;
    background-color: #4c84ff;
}

.actions {
    display: flex;
    gap: 8px;
}

.action-btn {
    padding: 4px 12px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
}

.action-btn.edit {
    background-color: #4c84ff;
    color: white;
}

.action-btn.delete {
    background-color: #ff4c4c;
    color: white;
}

.load-more {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

.more-btn {
    padding: 8px 24px;
    background-color: #f5f7fa;
    border: 1px solid #ddd;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
}

.more-btn:hover {
    background-color: #e6e8eb;
}
</style>