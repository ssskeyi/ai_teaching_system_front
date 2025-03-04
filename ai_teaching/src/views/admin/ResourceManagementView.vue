<template>
    <div class="resource-management">
        <h1>资源管理</h1>

        <div class="management-actions">
            <div class="search-bar">
                <input type="text" v-model="searchQuery" placeholder="搜索资源..." @input="searchResources" />
                <select v-model="typeFilter" @change="filterResourcesByType">
                    <option value="">所有类型</option>
                    <option value="document">文档</option>
                    <option value="video">视频</option>
                    <option value="audio">音频</option>
                    <option value="exercise">练习题</option>
                </select>
            </div>
            <button class="add-resource-btn" @click="showAddResourceModal = true">添加资源</button>
        </div>

        <div class="resources-table-container">
            <table class="resources-table">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>标题</th>
                        <th>类型</th>
                        <th>上传者</th>
                        <th>上传时间</th>
                        <th>状态</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="resource in displayedResources" :key="resource.id">
                        <td>{{ resource.id }}</td>
                        <td>{{ resource.title }}</td>
                        <td>{{ formatType(resource.type) }}</td>
                        <td>{{ resource.uploader }}</td>
                        <td>{{ formatDate(resource.uploadDate) }}</td>
                        <td>
                            <span :class="['status-badge', resource.approved ? 'approved' : 'pending']">
                                {{ resource.approved ? '已通过' : '待审核' }}
                            </span>
                        </td>
                        <td class="actions">
                            <button class="view-btn" @click="viewResource(resource)">查看</button>
                            <button class="approve-btn" v-if="!resource.approved" @click="approveResource(resource)">
                                通过
                            </button>
                            <button class="reject-btn" v-if="!resource.approved" @click="rejectResource(resource)">
                                拒绝
                            </button>
                            <button class="delete-btn" @click="deleteResource(resource)">删除</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- 添加资源对话框 -->
        <div v-if="showAddResourceModal" class="modal">
            <div class="modal-content">
                <h2>添加资源</h2>
                <form @submit.prevent="addResource">
                    <div class="form-group">
                        <label for="title">标题</label>
                        <input type="text" id="title" v-model="newResource.title" required />
                    </div>
                    <div class="form-group">
                        <label for="description">描述</label>
                        <textarea id="description" v-model="newResource.description" rows="4" required></textarea>
                    </div>
                    <div class="form-group">
                        <label for="type">类型</label>
                        <select id="type" v-model="newResource.type" required>
                            <option value="">请选择类型</option>
                            <option value="document">文档</option>
                            <option value="video">视频</option>
                            <option value="audio">音频</option>
                            <option value="exercise">练习题</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="file">上传文件</label>
                        <input type="file" id="file" />
                        <p class="file-hint">支持的格式：PDF, DOC, MP4, MP3</p>
                    </div>
                    <div class="modal-actions">
                        <button type="button" @click="showAddResourceModal = false">取消</button>
                        <button type="submit" class="save-btn">保存</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- 查看资源对话框 -->
        <div v-if="showViewResourceModal" class="modal">
            <div class="modal-content resource-details">
                <h2>资源详情</h2>
                <div class="resource-info">
                    <p><strong>标题：</strong>{{ viewingResource.title }}</p>
                    <p><strong>描述：</strong>{{ viewingResource.description }}</p>
                    <p><strong>类型：</strong>{{ formatType(viewingResource.type) }}</p>
                    <p><strong>上传者：</strong>{{ viewingResource.uploader }}</p>
                    <p><strong>上传时间：</strong>{{ formatDate(viewingResource.uploadDate) }}</p>
                    <p><strong>状态：</strong>{{ viewingResource.approved ? '已通过' : '待审核' }}</p>
                </div>
                <div class="resource-preview">
                    <h3>资源预览</h3>
                    <!-- 根据资源类型显示不同的预览 -->
                    <div v-if="viewingResource.type === 'document'" class="document-preview">
                        <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9ImN1cnJlbnRDb2xvciIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIGNsYXNzPSJmZWF0aGVyIGZlYXRoZXItZmlsZS10ZXh0Ij48cGF0aCBkPSJNMTQgMkg2YTIgMiAwIDAgMC0yIDJ2MTZhMiAyIDAgMCAwIDIgMmgxMmEyIDIgMCAwIDAgMi0yVjh6Ij48L3BhdGg+PHBvbHlsaW5lIHBvaW50cz0iMTQgMiAxNCA4IDIwIDgiPjwvcG9seWxpbmU+PGxpbmUgeDE9IjE2IiB5MT0iMTMiIHgyPSI4IiB5Mj0iMTMiPjwvbGluZT48bGluZSB4MT0iMTYiIHkxPSIxNyIgeDI9IjgiIHkyPSIxNyI+PC9saW5lPjxwb2x5bGluZSBwb2ludHM9IjEwIDkgOSA5IDggOSI+PC9wb2x5bGluZT48L3N2Zz4="
                            alt="文档图标" />
                        <p>文档预览不可用，请下载后查看</p>
                    </div>
                    <div v-else-if="viewingResource.type === 'video'" class="video-preview">
                        <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9ImN1cnJlbnRDb2xvciIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIGNsYXNzPSJmZWF0aGVyIGZlYXRoZXItdmlkZW8iPjxwb2x5Z29uIHBvaW50cz0iMjMgNyAxNiAxMiAyMyAxNyAyMyA3Ij48L3BvbHlnb24+PHJlY3QgeD0iMSIgeT0iNSIgd2lkdGg9IjE1IiBoZWlnaHQ9IjE0IiByeD0iMiIgcnk9IjIiPjwvcmVjdD48L3N2Zz4="
                            alt="视频图标" />
                        <p>视频预览不可用，请下载后查看</p>
                    </div>
                    <div v-else-if="viewingResource.type === 'audio'" class="audio-preview">
                        <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9ImN1cnJlbnRDb2xvciIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIGNsYXNzPSJmZWF0aGVyIGZlYXRoZXItbXVzaWMiPjxwYXRoIGQ9Ik05IDE4VjVsOSAtMXYxM1wiPjwvcGF0aD48Y2lyY2xlIGN4PSI2IiBjeT0iMTgiIHI9IjMiPjwvY2lyY2xlPjxjaXJjbGUgY3g9IjE4IiBjeT0iMTYiIHI9IjMiPjwvY2lyY2xlPjwvc3ZnPg=="
                            alt="音频图标" />
                        <p>音频预览不可用，请下载后查看</p>
                    </div>
                    <div v-else-if="viewingResource.type === 'exercise'" class="exercise-preview">
                        <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9ImN1cnJlbnRDb2xvciIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIGNsYXNzPSJmZWF0aGVyIGZlYXRoZXItY2hlY2stc3F1YXJlIj48cG9seWxpbmUgcG9pbnRzPSI5IDExIDEyIDE0IDIyIDQiPjwvcG9seWxpbmU+PHBhdGggZD0iTTIxIDEydjdhMiAyIDAgMCAxLTIgMkg1YTIgMiAwIDAgMS0yLTJWNWEyIDIgMCAwIDEgMi0yaDExIj48L3BhdGg+PC9zdmc+"
                            alt="练习题图标" />
                        <p>练习题预览不可用，请下载后查看</p>
                    </div>
                </div>
                <div class="modal-actions">
                    <button type="button" @click="showViewResourceModal = false">关闭</button>
                    <button class="download-btn">下载</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';

// 模拟资源数据
const resources = ref([
    {
        id: 1,
        title: '数学基础知识讲解',
        description: '包含高中数学必备知识点汇总和详细讲解',
        type: 'document',
        uploader: 'teacher1',
        uploadDate: new Date(2023, 5, 15),
        approved: true
    },
    {
        id: 2,
        title: '物理实验视频教程',
        description: '高中物理实验操作示范和注意事项',
        type: 'video',
        uploader: 'teacher2',
        uploadDate: new Date(2023, 6, 20),
        approved: true
    },
    {
        id: 3,
        title: '英语听力练习',
        description: '英语四级听力训练材料',
        type: 'audio',
        uploader: 'teacher1',
        uploadDate: new Date(2023, 7, 10),
        approved: false
    },
    {
        id: 4,
        title: '化学选择题集',
        description: '高考化学选择题汇编及解析',
        type: 'exercise',
        uploader: 'teacher3',
        uploadDate: new Date(2023, 8, 5),
        approved: false
    },
    {
        id: 5,
        title: '语文作文指导',
        description: '高中语文作文写作技巧和范文',
        type: 'document',
        uploader: 'teacher2',
        uploadDate: new Date(2023, 9, 1),
        approved: true
    },
]);

const searchQuery = ref('');
const typeFilter = ref('');
const showAddResourceModal = ref(false);
const showViewResourceModal = ref(false);
const viewingResource = ref<any>({});
const newResource = ref({
    title: '',
    description: '',
    type: ''
});

// 过滤显示的资源
const displayedResources = computed(() => {
    let filtered = resources.value;

    if (searchQuery.value) {
        const query = searchQuery.value.toLowerCase();
        filtered = filtered.filter(resource =>
            resource.title.toLowerCase().includes(query) ||
            resource.description.toLowerCase().includes(query)
        );
    }

    if (typeFilter.value) {
        filtered = filtered.filter(resource => resource.type === typeFilter.value);
    }

    return filtered;
});

// 格式化类型
const formatType = (type: string) => {
    const typeMap: Record<string, string> = {
        document: '文档',
        video: '视频',
        audio: '音频',
        exercise: '练习题'
    };
    return typeMap[type] || type;
};

// 格式化日期
const formatDate = (date: Date) => {
    return date.toLocaleDateString('zh-CN');
};

// 搜索资源
const searchResources = () => {
    // 实际应用中可能需要请求后端
    console.log('搜索资源:', searchQuery.value);
};

// 按类型过滤资源
const filterResourcesByType = () => {
    // 实际应用中可能需要请求后端
    console.log('按类型过滤:', typeFilter.value);
};

// 添加资源
const addResource = () => {
    // 实际应用中应发送请求到后端
    const newId = Math.max(...resources.value.map(r => r.id)) + 1;
    const resourceToAdd = {
        id: newId,
        title: newResource.value.title,
        description: newResource.value.description,
        type: newResource.value.type,
        uploader: 'admin1', // 当前登录用户
        uploadDate: new Date(),
        approved: true // 管理员上传自动通过
    };

    resources.value.push(resourceToAdd);
    showAddResourceModal.value = false;
    newResource.value = { title: '', description: '', type: '' };
};

// 查看资源
const viewResource = (resource: any) => {
    viewingResource.value = { ...resource };
    showViewResourceModal.value = true;
};

// 审核通过资源
const approveResource = (resource: any) => {
    // 实际应用中应发送请求到后端
    const index = resources.value.findIndex(r => r.id === resource.id);
    if (index !== -1) {
        resources.value[index].approved = true;
    }
};

// 拒绝资源
const rejectResource = (resource: any) => {
    // 实际应用中应发送请求到后端
    if (confirm(`确定要拒绝资源"${resource.title}"吗？`)) {
        const index = resources.value.findIndex(r => r.id === resource.id);
        if (index !== -1) {
            resources.value.splice(index, 1);
        }
    }
};

// 删除资源
const deleteResource = (resource: any) => {
    // 实际应用中应发送请求到后端
    if (confirm(`确定要删除资源"${resource.title}"吗？`)) {
        resources.value = resources.value.filter(r => r.id !== resource.id);
    }
};

onMounted(() => {
    // 在实际应用中，这里应该从API获取资源列表
    console.log('资源管理组件已挂载');
});
</script>

<style scoped>
.resource-management {
    padding: 1rem;
}

h1 {
    margin-bottom: 2rem;
    color: #2c3e50;
}

.management-actions {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1.5rem;
}

.search-bar {
    display: flex;
    gap: 1rem;
    flex: 1;
    max-width: 500px;
}

.search-bar input,
.search-bar select {
    padding: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.search-bar input {
    flex: 1;
}

.add-resource-btn {
    background-color: #2ecc71;
    color: white;
    border: none;
    border-radius: 4px;
    padding: 0.5rem 1rem;
    cursor: pointer;
    transition: background-color 0.3s;
}

.add-resource-btn:hover {
    background-color: #27ae60;
}

.resources-table-container {
    overflow-x: auto;
}

.resources-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
}

.resources-table th,
.resources-table td {
    border: 1px solid #ddd;
    padding: 0.75rem;
    text-align: left;
}

.resources-table th {
    background-color: #f5f7fa;
    color: #2c3e50;
    font-weight: 600;
}

.resources-table tr:nth-child(even) {
    background-color: #f9f9f9;
}

.resources-table tr:hover {
    background-color: #f1f1f1;
}

.status-badge {
    display: inline-block;
    padding: 0.25rem 0.5rem;
    border-radius: 20px;
    font-size: 0.85rem;
}

.status-badge.approved {
    background-color: #d5f5e3;
    color: #27ae60;
}

.status-badge.pending {
    background-color: #fef9e7;
    color: #f39c12;
}

.actions {
    display: flex;
    gap: 0.5rem;
}

.actions button {
    padding: 0.25rem 0.5rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
}

.view-btn {
    background-color: #3498db;
    color: white;
}

.view-btn:hover {
    background-color: #2980b9;
}

.approve-btn {
    background-color: #2ecc71;
    color: white;
}

.approve-btn:hover {
    background-color: #27ae60;
}

.reject-btn {
    background-color: #f39c12;
    color: white;
}

.reject-btn:hover {
    background-color: #d35400;
}

.delete-btn {
    background-color: #e74c3c;
    color: white;
}

.delete-btn:hover {
    background-color: #c0392b;
}

.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}

.modal-content {
    background-color: white;
    padding: 2rem;
    border-radius: 8px;
    width: 100%;
    max-width: 500px;
    max-height: 90vh;
    overflow-y: auto;
}

.modal h2 {
    margin-top: 0;
    margin-bottom: 1.5rem;
    color: #2c3e50;
}

.form-group {
    margin-bottom: 1.5rem;
}

.form-group label {
    display: block;
    margin-bottom: 0.5rem;
    color: #34495e;
}

.form-group input,
.form-group select,
.form-group textarea {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.file-hint {
    font-size: 0.85rem;
    color: #7f8c8d;
    margin-top: 0.5rem;
}

.modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: 1rem;
    margin-top: 2rem;
}

.modal-actions button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.save-btn {
    background-color: #2ecc71;
    color: white;
}

.save-btn:hover {
    background-color: #27ae60;
}

.download-btn {
    background-color: #3498db;
    color: white;
}

.download-btn:hover {
    background-color: #2980b9;
}

.resource-details {
    max-width: 600px;
}

.resource-info {
    margin-bottom: 1.5rem;
}

.resource-info p {
    margin: 0.5rem 0;
}

.resource-preview {
    border-top: 1px solid #ddd;
    padding-top: 1.5rem;
    margin-top: 1.5rem;
}

.resource-preview h3 {
    margin-top: 0;
    margin-bottom: 1rem;
    color: #2c3e50;
}

.document-preview,
.video-preview,
.audio-preview,
.exercise-preview {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    background-color: #f9f9f9;
    border-radius: 4px;
}

.document-preview img,
.video-preview img,
.audio-preview img,
.exercise-preview img {
    width: 48px;
    height: 48px;
    margin-bottom: 1rem;
    opacity: 0.7;
}

.document-preview p,
.video-preview p,
.audio-preview p,
.exercise-preview p {
    color: #7f8c8d;
    font-style: italic;
}
</style>