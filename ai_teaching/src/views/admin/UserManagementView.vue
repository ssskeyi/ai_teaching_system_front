<template>
    <div class="user-management">
        <h1>用户管理</h1>

        <div class="management-actions">
            <div class="search-bar">
                <input type="text" v-model="searchQuery" placeholder="搜索用户..." @input="searchUsers" />
                <select v-model="roleFilter" @change="filterUsersByRole">
                    <option value="">所有角色</option>
                    <option value="teacher">教师</option>
                    <option value="student">学生</option>
                    <option value="admin">管理员</option>
                </select>
            </div>
            <button class="add-user-btn" @click="showAddUserModal = true">添加用户</button>
        </div>

        <div class="users-table-container">
            <table class="users-table">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>用户名</th>
                        <th>邮箱</th>
                        <th>角色</th>
                        <th>状态</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="user in displayedUsers" :key="user.id">
                        <td>{{ user.id }}</td>
                        <td>{{ user.username }}</td>
                        <td>{{ user.email }}</td>
                        <td>{{ formatRole(user.role) }}</td>
                        <td>
                            <span :class="['status-badge', user.active ? 'active' : 'inactive']">
                                {{ user.active ? '活跃' : '禁用' }}
                            </span>
                        </td>
                        <td class="actions">
                            <button class="edit-btn" @click="editUser(user)">编辑</button>
                            <button class="toggle-status-btn" @click="toggleUserStatus(user)">
                                {{ user.active ? '禁用' : '启用' }}
                            </button>
                            <button class="delete-btn" @click="deleteUser(user)">删除</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- 添加用户对话框 -->
        <div v-if="showAddUserModal" class="modal">
            <div class="modal-content">
                <h2>添加用户</h2>
                <form @submit.prevent="addUser">
                    <div class="form-group">
                        <label for="username">用户名</label>
                        <input type="text" id="username" v-model="newUser.username" required />
                    </div>
                    <div class="form-group">
                        <label for="email">邮箱</label>
                        <input type="email" id="email" v-model="newUser.email" required />
                    </div>
                    <div class="form-group">
                        <label for="password">密码</label>
                        <input type="password" id="password" v-model="newUser.password" required />
                    </div>
                    <div class="form-group">
                        <label for="role">角色</label>
                        <select id="role" v-model="newUser.role" required>
                            <option value="">请选择角色</option>
                            <option value="teacher">教师</option>
                            <option value="student">学生</option>
                            <option value="admin">管理员</option>
                        </select>
                    </div>
                    <div class="modal-actions">
                        <button type="button" @click="showAddUserModal = false">取消</button>
                        <button type="submit" class="save-btn">保存</button>
                    </div>
                </form>
            </div>
        </div>

        <!-- 编辑用户对话框 -->
        <div v-if="showEditUserModal" class="modal">
            <div class="modal-content">
                <h2>编辑用户</h2>
                <form @submit.prevent="updateUser">
                    <div class="form-group">
                        <label for="edit-username">用户名</label>
                        <input type="text" id="edit-username" v-model="editingUser.username" required />
                    </div>
                    <div class="form-group">
                        <label for="edit-email">邮箱</label>
                        <input type="email" id="edit-email" v-model="editingUser.email" required />
                    </div>
                    <div class="form-group">
                        <label for="edit-role">角色</label>
                        <select id="edit-role" v-model="editingUser.role" required>
                            <option value="">请选择角色</option>
                            <option value="teacher">教师</option>
                            <option value="student">学生</option>
                            <option value="admin">管理员</option>
                        </select>
                    </div>
                    <div class="modal-actions">
                        <button type="button" @click="showEditUserModal = false">取消</button>
                        <button type="submit" class="save-btn">保存</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';

// 模拟用户数据
const users = ref([
    { id: 1, username: 'teacher1', email: 'teacher1@example.com', role: 'teacher', active: true },
    { id: 2, username: 'student1', email: 'student1@example.com', role: 'student', active: true },
    { id: 3, username: 'admin1', email: 'admin1@example.com', role: 'admin', active: true },
    { id: 4, username: 'teacher2', email: 'teacher2@example.com', role: 'teacher', active: false },
    { id: 5, username: 'student2', email: 'student2@example.com', role: 'student', active: true },
]);

const searchQuery = ref('');
const roleFilter = ref('');
const showAddUserModal = ref(false);
const showEditUserModal = ref(false);
const editingUser = ref<any>(null);
const newUser = ref({
    username: '',
    email: '',
    password: '',
    role: ''
});

// 过滤显示的用户
const displayedUsers = computed(() => {
    let filtered = users.value;

    if (searchQuery.value) {
        const query = searchQuery.value.toLowerCase();
        filtered = filtered.filter(user =>
            user.username.toLowerCase().includes(query) ||
            user.email.toLowerCase().includes(query)
        );
    }

    if (roleFilter.value) {
        filtered = filtered.filter(user => user.role === roleFilter.value);
    }

    return filtered;
});

// 角色格式化
const formatRole = (role: string) => {
    const roleMap: Record<string, string> = {
        teacher: '教师',
        student: '学生',
        admin: '管理员'
    };
    return roleMap[role] || role;
};

// 搜索用户
const searchUsers = () => {
    // 实际应用中可能需要请求后端
    console.log('搜索用户:', searchQuery.value);
};

// 按角色过滤用户
const filterUsersByRole = () => {
    // 实际应用中可能需要请求后端
    console.log('按角色过滤:', roleFilter.value);
};

// 添加用户
const addUser = () => {
    // 实际应用中应发送请求到后端
    const newId = Math.max(...users.value.map(u => u.id)) + 1;
    const userToAdd = {
        id: newId,
        username: newUser.value.username,
        email: newUser.value.email,
        role: newUser.value.role,
        active: true
    };

    users.value.push(userToAdd);
    showAddUserModal.value = false;
    newUser.value = { username: '', email: '', password: '', role: '' };
};

// 编辑用户
const editUser = (user: any) => {
    editingUser.value = { ...user };
    showEditUserModal.value = true;
};

// 更新用户
const updateUser = () => {
    // 实际应用中应发送请求到后端
    const index = users.value.findIndex(u => u.id === editingUser.value.id);
    if (index !== -1) {
        users.value[index] = { ...editingUser.value };
    }
    showEditUserModal.value = false;
};

// 切换用户状态（启用/禁用）
const toggleUserStatus = (user: any) => {
    // 实际应用中应发送请求到后端
    const index = users.value.findIndex(u => u.id === user.id);
    if (index !== -1) {
        users.value[index].active = !users.value[index].active;
    }
};

// 删除用户
const deleteUser = (user: any) => {
    // 实际应用中应发送请求到后端
    if (confirm(`确定要删除用户 ${user.username} 吗？`)) {
        users.value = users.value.filter(u => u.id !== user.id);
    }
};

onMounted(() => {
    // 在实际应用中，这里应该从API获取用户列表
    console.log('用户管理组件已挂载');
});
</script>

<style scoped>
.user-management {
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

.add-user-btn {
    background-color: #2ecc71;
    color: white;
    border: none;
    border-radius: 4px;
    padding: 0.5rem 1rem;
    cursor: pointer;
    transition: background-color 0.3s;
}

.add-user-btn:hover {
    background-color: #27ae60;
}

.users-table-container {
    overflow-x: auto;
}

.users-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
}

.users-table th,
.users-table td {
    border: 1px solid #ddd;
    padding: 0.75rem;
    text-align: left;
}

.users-table th {
    background-color: #f5f7fa;
    color: #2c3e50;
    font-weight: 600;
}

.users-table tr:nth-child(even) {
    background-color: #f9f9f9;
}

.users-table tr:hover {
    background-color: #f1f1f1;
}

.status-badge {
    display: inline-block;
    padding: 0.25rem 0.5rem;
    border-radius: 20px;
    font-size: 0.85rem;
}

.status-badge.active {
    background-color: #d5f5e3;
    color: #27ae60;
}

.status-badge.inactive {
    background-color: #fadbd8;
    color: #e74c3c;
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

.edit-btn {
    background-color: #3498db;
    color: white;
}

.edit-btn:hover {
    background-color: #2980b9;
}

.toggle-status-btn {
    background-color: #f39c12;
    color: white;
}

.toggle-status-btn:hover {
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
.form-group select {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #ddd;
    border-radius: 4px;
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
</style>