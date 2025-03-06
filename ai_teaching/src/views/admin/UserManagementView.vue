<template>
    <div class="user-management">
        <h2>用户管理</h2>

        <!-- 搜索和操作区 -->
        <div class="top-actions">
            <div class="search-area">
                <input type="text" v-model="searchQuery" placeholder="搜索..." class="search-input" />
                <button class="search-btn">搜索</button>
            </div>
            <button class="add-user-btn" @click="showAddUserModal = true">添加用户</button>
        </div>

        <!-- 用户类型筛选 -->
        <div class="filter-tabs">
            <button v-for="type in userTypes" :key="type.value"
                :class="['filter-tab', { active: selectedType === type.value }]" @click="selectedType = type.value">
                {{ type.label }}
            </button>
        </div>

        <!-- 用户列表 -->
        <div class="user-table">
            <table>
                <thead>
                    <tr>
                        <th>用户名</th>
                        <th>类别</th>
                        <th>用户状态</th>
                        <th>学工号</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="user in filteredUsers" :key="user.id">
                        <td>{{ user.username }}</td>
                        <td>{{ user.type === 'teacher' ? '教师' :
                            user.type === 'student' ? '学生' :
                                user.type === 'admin' ? '管理员' : user.type }}</td>
                        <td>
                            <span :class="['status-tag', user.status]">
                                {{ user.status }}
                            </span>
                        </td>
                        <td>{{ user.number }}</td>
                        <td class="actions">
                            <button class="action-btn" :class="user.status === '已禁用' ? 'restore' : 'ban'"
                                @click="toggleUserStatus(user)">
                                {{ user.status === '已禁用' ? '恢复' : '禁用' }}
                            </button>
                            <button class="action-btn reset">重置密码</button>
                            <button class="action-btn delete">注销</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- 添加用户对话框 -->
        <div v-if="showAddUserModal" class="modal">
            <div class="modal-content">
                <h3>添加用户</h3>
                <form @submit.prevent="addUser">
                    <div class="form-group">
                        <label>用户名</label>
                        <input type="text" v-model="newUser.username" required />
                    </div>
                    <div class="form-group">
                        <label>类别</label>
                        <select v-model="newUser.type" required>
                            <option value="">请选择类别</option>
                            <option value="teacher">教师</option>
                            <option value="student">学生</option>
                            <option value="admin">管理员</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>学工号</label>
                        <input type="text" v-model="newUser.number" required />
                    </div>
                    <div class="form-group">
                        <label>初始密码</label>
                        <input type="password" v-model="newUser.password" required />
                    </div>
                    <div class="modal-actions">
                        <button type="button" class="cancel-btn" @click="showAddUserModal = false">取消</button>
                        <button type="submit" class="submit-btn">确定</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const searchQuery = ref('');
const selectedType = ref('all');
const showAddUserModal = ref(false);

// 用户类型选项
const userTypes = [
    { label: '全部', value: 'all' },
    { label: '教师', value: 'teacher' },
    { label: '学生', value: 'student' },
    { label: '管理员', value: 'admin' }
];

// 新用户表单数据
const newUser = ref({
    username: '',
    type: '',
    number: '',
    password: ''
});

// 模拟用户数据
const users = ref([
    { id: 1, username: '林雨廷', type: 'student', status: '正常', number: '20225789' },
    { id: 2, username: '徐硕', type: 'student', status: '正常', number: '20222313' },
    { id: 3, username: '李木', type: 'admin', status: '正常', number: '243452' },
    { id: 4, username: '欧阳子七', type: 'teacher', status: '已禁用', number: '34785678' }
]);

// 根据筛选条件过滤用户
const filteredUsers = computed(() => {
    return users.value.filter(user => {
        const matchesSearch = user.username.includes(searchQuery.value) ||
            user.number.includes(searchQuery.value);
        const matchesType = selectedType.value === 'all' || user.type === selectedType.value;
        return matchesSearch && matchesType;
    });
});

// 添加用户
const addUser = () => {
    const newUserData = {
        id: users.value.length + 1,
        ...newUser.value,
        status: '正常'
    };
    users.value.push(newUserData);
    showAddUserModal.value = false;
    newUser.value = { username: '', type: '', number: '', password: '' };
};

// 切换用户状态
const toggleUserStatus = (user: any) => {
    user.status = user.status === '正常' ? '已禁用' : '正常';
};
</script>

<style scoped>
.user-management {
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

.add-user-btn {
    padding: 8px 20px;
    background-color: #4c84ff;
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
    padding: 8px 20px;
    background-color: #f5f7fa;
    border: none;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
    transition: all 0.3s;
}

.filter-tab.active {
    background-color: #4c84ff;
    color: white;
}

.user-table {
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

.status-tag {
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 12px;
}

.status-tag.正常 {
    background-color: #e3f2fd;
    color: #4c84ff;
}

.status-tag.已禁用 {
    background-color: #ffebee;
    color: #dc3545;
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

.action-btn.ban {
    background-color: #4c84ff;
}

.action-btn.restore {
    background-color: #28a745;
}

.action-btn.reset {
    background-color: #ffc107;
}

.action-btn.delete {
    background-color: #dc3545;
}

/* 模态框样式 */
.modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}

.modal-content {
    background: white;
    padding: 24px;
    border-radius: 8px;
    width: 90%;
    max-width: 400px;
}

.modal-content h3 {
    margin: 0 0 20px 0;
    color: #333;
}

.form-group {
    margin-bottom: 16px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    color: #666;
}

.form-group input,
.form-group select {
    width: 100%;
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: 12px;
    margin-top: 24px;
}

.cancel-btn {
    padding: 8px 20px;
    background-color: #f5f7fa;
    border: 1px solid #ddd;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
}

.submit-btn {
    padding: 8px 20px;
    background-color: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
</style>