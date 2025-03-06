<template>
    <div class="resource-management">
        <h2>资源管理</h2>

        <!-- 搜索和操作区 -->
        <div class="top-actions">
            <div class="search-area">
                <input type="text" v-model="searchQuery" placeholder="搜索..." class="search-input" />
                <button class="search-btn">搜索</button>
            </div>
        </div>

        <!-- 资源状态筛选 -->
        <div class="filter-tabs">
            <button v-for="status in resourceStatuses" :key="status.value"
                :class="['filter-tab', { active: selectedStatus === status.value }]"
                @click="selectedStatus = status.value">
                {{ status.label }}
            </button>
        </div>

        <!-- 资源类型筛选 -->
        <div class="type-filter">
            <button v-for="type in resourceTypes" :key="type.value"
                :class="['type-btn', { active: selectedType === type.value }]" @click="selectedType = type.value">
                {{ type.label }}
            </button>
        </div>

        <!-- 资源列表 -->
        <div class="resource-table">
            <table>
                <thead>
                    <tr>
                        <th>名称</th>
                        <th>类型</th>
                        <th>上传时间</th>
                        <th>上传者</th>
                        <th>状态</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="resource in filteredResources" :key="resource.id">
                        <td>{{ resource.name }}</td>
                        <td>
                            <span class="type-tag">{{ resource.type }}</span>
                        </td>
                        <td>{{ resource.date }}</td>
                        <td>{{ resource.uploader }}</td>
                        <td>
                            <span :class="['status-tag', resource.status]">
                                {{ resource.status }}
                            </span>
                        </td>
                        <td class="actions">
                            <button v-if="resource.status === '待审核'" class="action-btn approve"
                                @click="approveResource(resource)">
                                审核
                            </button>
                            <button v-if="resource.status === '已审核'" class="action-btn share">
                                共享
                            </button>
                            <button class="action-btn delete">删除</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const searchQuery = ref('');
const selectedStatus = ref('');
const selectedType = ref('');

// 资源状态选项
const resourceStatuses = [
    { label: '全部', value: '' },
    { label: '待审核', value: '待审核' },
    { label: '已审核', value: '已审核' }
];

// 资源类型选项
const resourceTypes = [
    { label: '课件', value: 'courseware' },
    { label: '习题', value: 'exercise' },
    { label: '文档', value: 'document' },
    { label: '教案', value: 'lesson' },
    { label: '链接', value: 'link' },
    { label: '视频', value: 'video' },
    { label: '图片', value: 'image' }
];

// 模拟资源数据
const resources = ref([
    {
        id: 1,
        name: '2024年高等数学上期中模考题（三）',
        type: '习题',
        date: '2024/5/27',
        uploader: '张老师',
        status: '待审核'
    },
    {
        id: 2,
        name: '关于......的文献研究',
        type: '文档',
        date: '2024/3/27',
        uploader: '李老师',
        status: '已审核'
    },
    {
        id: 3,
        name: '大物练习题',
        type: '习题',
        date: '2024/1/2',
        uploader: '王老师',
        status: '待审核'
    },
    {
        id: 4,
        name: '高等数学第三章PPT',
        type: '课件',
        date: '2023/9/10',
        uploader: '赵老师',
        status: '已审核'
    }
]);

// 根据筛选条件过滤资源
const filteredResources = computed(() => {
    return resources.value.filter(resource => {
        const matchesSearch = resource.name.includes(searchQuery.value) ||
            resource.uploader.includes(searchQuery.value);
        const matchesStatus = !selectedStatus.value || resource.status === selectedStatus.value;
        const matchesType = !selectedType.value || resource.type === resourceTypes.find(t => t.value === selectedType.value)?.label;
        return matchesSearch && matchesStatus && matchesType;
    });
});

// 审核资源
const approveResource = (resource: any) => {
    resource.status = '已审核';
};
</script>

<style scoped>
.resource-management {
    padding: 20px;
}

h2 {
    margin-bottom: 20px;
    color: #333;
}

.top-actions {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}

.search-area {
    display: flex;
    gap: 10px;
}

.search-input {
    width: 300px;
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.search-btn {
    padding: 8px 20px;
    background-color: #666;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.filter-tabs {
    margin-bottom: 20px;
    display: flex;
    gap: 10px;
}

.filter-tab {
    padding: 6px 16px;
    background-color: #f5f7fa;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    color: #666;
}

.filter-tab.active {
    background-color: #4c84ff;
    color: white;
}

.type-filter {
    margin-bottom: 20px;
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.type-btn {
    padding: 6px 16px;
    background-color: #f5f7fa;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    color: #666;
}

.type-btn.active {
    background-color: #4c84ff;
    color: white;
}

.resource-table {
    background: white;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #eee;
}

th {
    color: #666;
    font-weight: normal;
}

.type-tag {
    display: inline-block;
    padding: 2px 8px;
    background-color: #e3f2fd;
    color: #4c84ff;
    border-radius: 4px;
    font-size: 12px;
}

.status-tag {
    display: inline-block;
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 12px;
}

.status-tag.待审核 {
    background-color: #fff3e0;
    color: #ff9800;
}

.status-tag.已审核 {
    background-color: #e8f5e9;
    color: #4caf50;
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
    color: white;
}

.action-btn.approve {
    background-color: #4c84ff;
}

.action-btn.share {
    background-color: #4caf50;
}

.action-btn.delete {
    background-color: #dc3545;
}
</style>