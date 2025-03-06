<template>
    <div class="classes-index">
        <div class="header">
            <h2>我的班级</h2>
        </div>

        <div class="class-cards">
            <div v-for="classItem in classes" :key="classItem.id" class="class-card">
                <div class="class-info">
                    <h3>{{ classItem.name }}</h3>
                    <div class="info-row">
                        <span class="label">教授科目：</span>
                        <span class="value">{{ classItem.subjects }}</span>
                        <span class="status" :class="classItem.status">{{ classItem.status }}</span>
                    </div>
                    <div class="info-row">
                        <span class="label">学生人数：</span>
                        <span class="value">{{ classItem.studentCount }}</span>
                    </div>
                </div>
                <button class="enter-btn" @click="enterClass(classItem.id)">进入班级</button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// 模拟班级数据
const classes = ref([
    {
        id: 1,
        name: '软件2202班',
        subjects: '数据结构',
        studentCount: 45,
        status: '进行中'
    },
    {
        id: 2,
        name: '软件2203班',
        subjects: '数据结构',
        studentCount: 41,
        status: '进行中'
    },
    {
        id: 3,
        name: '集成2307班',
        subjects: 'C语言、嵌入式',
        studentCount: 31,
        status: '进行中'
    },
    {
        id: 4,
        name: '软件2105班',
        subjects: '数据结构',
        studentCount: 40,
        status: '已结束'
    },
    {
        id: 5,
        name: '微电2105班',
        subjects: '数字电路设计',
        studentCount: 42,
        status: '已结束'
    }
]);

// 进入班级
const enterClass = (classId: number) => {
    router.push(`/student/classes/${classId}`);
};
</script>

<style scoped>
.classes-index {
    padding: 20px;
    height: 100%;
        display: flex;
        flex-direction: column;
        gap: 20px;
    }
    
    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    
    h2 {
        font-size: 20px;
        color: #333;
        margin: 0;
    }
    
    .class-cards {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        gap: 20px;
        padding: 10px 0;
    }
    
    .class-card {
        background: white;
        border-radius: 8px;
        padding: 20px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        gap: 16px;
    }
    
    .class-info h3 {
        margin: 0 0 16px 0;
        font-size: 18px;
        color: #333;
    }
    
    .info-row {
        display: flex;
        align-items: center;
        margin-bottom: 8px;
    }
    
    .label {
        color: #666;
        font-size: 14px;
        width: 80px;
    }
    
    .value {
        color: #333;
        font-size: 14px;
        flex: 1;
    }
    
    .status {
        padding: 2px 8px;
        border-radius: 4px;
        font-size: 12px;
        color: white;
    }
    
    .status.进行中 {
        background-color: #4c84ff;
    }
    
    .status.已结束 {
        background-color: #999;
    }
    
    .enter-btn {
        width: 100%;
        padding: 8px;
        background-color: #4c84ff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: all 0.3s;
    }
    
    .enter-btn:hover {
        background-color: #3a70e3;
}
</style>