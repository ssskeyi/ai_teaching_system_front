<template>
    <div class="class-detail">
        <!-- 班级基本信息 -->
        <div class="class-header">
            <div class="class-info">
                <h2>{{ classInfo.name }}</h2>
                <div class="info-items">
                    <span>学生人数：{{ classInfo.studentCount }}</span>
                    <span>教授科目：{{ classInfo.subject }}</span>
                </div>
            </div>
            <button class="end-class-btn" @click="endClass" v-if="!classInfo.isEnded">结束授课</button>
        </div>

        <!-- 学生管理 -->
        <div class="section student-management">
            <div class="section-header">
                <h3>学生管理</h3>
                <button class="add-btn" @click="showAddStudentModal = true">添加学生</button>
            </div>
            <div class="table-container">
                <table>
                    <thead>
                        <tr>
                            <th>姓名</th>
                            <th>学号</th>
                            <th>操作</th>
                            <th>姓名</th>
                            <th>学号</th>
                            <th>操作</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(row, index) in studentRows" :key="index">
                            <template v-for="student in row" :key="student.id">
                                <td>{{ student.name }}</td>
                                <td>{{ student.studentId }}</td>
                                <td>
                                    <button class="remove-btn" @click="removeStudent(student.id)">移除</button>
                                </td>
                            </template>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- 作业管理 -->
        <div class="section homework-management">
            <h3>作业管理</h3>
            <div class="homework-list">
                <div v-for="homework in homeworkList" :key="homework.id" class="homework-item">
                    <div class="homework-info">
                        <span class="homework-name">{{ homework.name }}</span>
                        <span class="homework-status" :class="homework.status">{{ homework.status }}</span>
                        <span class="completion-rate">{{ homework.completionRate }}完成</span>
                    </div>
                    <div class="homework-actions">
                        <button class="action-btn" :class="homework.status === '已结束' ? 'view' : 'grade'"
                            @click="handleHomework(homework)">
                            {{ homework.status === '已结束' ? '查看' : '批阅' }}
                        </button>
                        <button class="action-btn total" @click="showTotalScore(homework)">总分</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- 学情分析 -->
        <div class="section learning-analysis">
            <h3>学情分析</h3>
            <div class="analysis-content">
                <!-- 这里可以添加图表等分析内容 -->
                <p>学情分析内容将在后续开发中添加...</p>
            </div>
        </div>

        <!-- 添加学生对话框 -->
        <div v-if="showAddStudentModal" class="modal">
            <div class="modal-content">
                <h3>添加学生</h3>
                <form @submit.prevent="addStudent">
                    <div class="form-group">
                        <label for="studentName">姓名</label>
                        <input type="text" id="studentName" v-model="newStudent.name" required />
                    </div>
                    <div class="form-group">
                        <label for="studentId">学号</label>
                        <input type="text" id="studentId" v-model="newStudent.studentId" required />
                    </div>
                    <div class="modal-actions">
                        <button type="button" class="cancel-btn" @click="showAddStudentModal = false">取消</button>
                        <button type="submit" class="submit-btn">确定</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const showAddStudentModal = ref(false);

// 班级基本信息
const classInfo = ref({
    id: route.params.id,
    name: '软件2202班',
    studentCount: 45,
    subject: '数据结构',
    isEnded: false
});

// 学生数据
const students = ref([
    { id: 1, name: '林雨廷', studentId: '20225789' },
    { id: 2, name: '徐硕', studentId: '20222313' },
    { id: 3, name: '李子木', studentId: '20213452' },
    { id: 4, name: 'XXX', studentId: '2022xxxx' },
    { id: 5, name: 'XXXX', studentId: '2022xxxx' },
    { id: 6, name: 'XX', studentId: '20xxxx52' }
]);

// 将学生数据转换为两列显示
const studentRows = computed(() => {
    const rows = [];
    for (let i = 0; i < students.value.length; i += 2) {
        rows.push(students.value.slice(i, i + 2));
    }
    return rows;
});

// 作业列表
const homeworkList = ref([
    { id: 1, name: '第二学期第五次测试', status: '进行中', completionRate: '30/45' },
    { id: 2, name: '第二学期第四次测试', status: '进行中', completionRate: '44/45' },
    { id: 3, name: '期中测试', status: '已结束', completionRate: '45/45' },
    { id: 4, name: '第一学期第二次测试', status: '已结束', completionRate: '40/45' }
]);

// 新学生表单数据
const newStudent = ref({
    name: '',
    studentId: ''
});

// 添加学生
const addStudent = () => {
    const newStudentData = {
        id: students.value.length + 1,
        name: newStudent.value.name,
        studentId: newStudent.value.studentId
    };
    students.value.push(newStudentData);
    classInfo.value.studentCount++;

    // 重置表单并关闭对话框
    newStudent.value = { name: '', studentId: '' };
    showAddStudentModal.value = false;
};

// 移除学生
const removeStudent = (studentId: number) => {
    const index = students.value.findIndex(s => s.id === studentId);
    if (index !== -1) {
        students.value.splice(index, 1);
        classInfo.value.studentCount--;
    }
};

// 结束授课
const endClass = () => {
    if (confirm('确定要结束授课吗？')) {
        classInfo.value.isEnded = true;
    }
};

// 处理作业
const handleHomework = (homework: any) => {
    console.log('处理作业:', homework);
};

// 显示总分
const showTotalScore = (homework: any) => {
    console.log('显示总分:', homework);
};
</script>

<style scoped>
.class-detail {
    padding: 20px;
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.class-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}

.class-info h2 {
    margin: 0 0 8px 0;
    font-size: 24px;
    color: #333;
}

.info-items {
    display: flex;
    gap: 20px;
    color: #666;
}

.end-class-btn {
    padding: 8px 20px;
    background-color: #dc3545;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.section {
    background: white;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
}

.section h3 {
    margin: 0;
    font-size: 18px;
    color: #333;
}

.add-btn {
    padding: 6px 16px;
    background-color: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.table-container {
    overflow-x: auto;
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
    font-weight: normal;
    color: #666;
    background-color: #f8f9fa;
}

.remove-btn {
    padding: 4px 12px;
    background-color: #dc3545;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.homework-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.homework-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px;
    background: #f8f9fa;
    border-radius: 4px;
}

.homework-info {
    display: flex;
    align-items: center;
    gap: 16px;
}

.homework-name {
    color: #333;
}

.homework-status {
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 12px;
    color: white;
}

.homework-status.进行中 {
    background-color: #4c84ff;
}

.homework-status.已结束 {
    background-color: #999;
}

.completion-rate {
    color: #666;
    font-size: 14px;
}

.homework-actions {
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

.action-btn.grade {
    background-color: #4c84ff;
}

.action-btn.view {
    background-color: #666;
}

.action-btn.total {
    background-color: #28a745;
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
    font-size: 18px;
    color: #333;
}

.form-group {
    margin-bottom: 16px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    color: #666;
    font-size: 14px;
}

.form-group input {
    width: 100%;
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 14px;
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

.submit-btn:hover {
    background-color: #3a70e3;
}

.learning-analysis {
    min-height: 200px;
}
</style>