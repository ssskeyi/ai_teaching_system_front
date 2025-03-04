<template>
    <div class="step1-view">
        <div class="main-content">
            <div class="form-section">
                <h2>请填写课程基础信息：</h2>
                <div class="form-content">
                    <div class="form-item">
                        <label>课程名称：</label>
                        <input type="text" v-model="courseInfo.name" placeholder="请输入课程名称" />
                    </div>
                    <div class="form-item">
                        <label>教学主题：</label>
                        <textarea v-model="courseInfo.theme" placeholder="请输入教学主题"></textarea>
                    </div>
                    <div class="form-item">
                        <label>教学目标：</label>
                        <textarea v-model="courseInfo.objective" placeholder="请输入教学目标"></textarea>
                    </div>
                    <div class="form-row">
                        <div class="form-item half-width">
                            <label>目标班级：</label>
                            <select v-model="courseInfo.targetClass">
                                <option value="">请选择目标班级</option>
                                <option v-for="grade in grades" :key="grade" :value="grade">{{ grade }}</option>
                            </select>
                        </div>
                        <div class="form-item half-width">
                            <label>课时长度：</label>
                            <div class="duration-input">
                                <select v-model="selectedDuration" @change="handleDurationChange">
                                    <option value="">请选择课时长度</option>
                                    <option v-for="duration in durations" :key="duration" :value="duration">
                                        {{ duration }}
                                    </option>
                                    <option value="custom">自定义</option>
                                </select>
                                <div v-if="isCustomDuration" class="custom-duration">
                                    <input type="number" v-model="customDurationValue" min="1"
                                        @input="handleCustomDurationInput" />
                                    <span class="unit">分钟</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="actions">
                <button class="action-btn back-btn" @click="goBack">上一步</button>
                <button class="action-btn start-btn" @click="startPreparation">下一步</button>
            </div>
        </div>

        <div class="side-panel">
            <div class="features-section">
                <h2>功能开关：</h2>
                <div class="features-content">
                    <div class="feature-row">
                        <div class="feature-item">
                            <span class="feature-label">资源推荐</span>
                            <div class="toggle-switch">
                                <input type="checkbox" v-model="features.resourceRecommend" :id="'resource'" />
                                <label :for="'resource'" class="switch"></label>
                            </div>
                        </div>
                        <div class="feature-item">
                            <span class="feature-label">练习题生成</span>
                            <div class="toggle-switch">
                                <input type="checkbox" v-model="features.exerciseGeneration" :id="'exercise'" />
                                <label :for="'exercise'" class="switch"></label>
                            </div>
                        </div>
                    </div>

                    <div class="special-requirements">
                        <h3>特殊教学需求：</h3>
                        <div class="tag-group">
                            <div v-for="(tag, index) in tags" :key="index" class="tag"
                                :class="{ active: selectedTags.includes(tag) }" @click="toggleTag(tag)">
                                {{ tag }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive, watch } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// 课程信息
const courseInfo = reactive({
    name: '',
    theme: '',
    objective: '',
    targetClass: '',
    duration: ''
});

// 年级选项
const grades = [
    '软2202',
    '集成2307',
    '软国2301',
    '微电2105'
];

// 课时长度选项
const durations = [
    '30分钟',
    '45分钟',
    '60分钟',
    '90分钟'
];

// 功能开关状态
const features = reactive({
    resourceRecommend: false,
    exerciseGeneration: false
});

// 标签选项
const tags = ['标签一', '标签二', '标签三', '标签四', '标签五', '标签六'];
const selectedTags = ref<string[]>([]);

// 切换标签选中状态
const toggleTag = (tag: string) => {
    const index = selectedTags.value.indexOf(tag);
    if (index === -1) {
        selectedTags.value.push(tag);
    } else {
        selectedTags.value.splice(index, 1);
    }
};

// 课时长度相关
const selectedDuration = ref('');
const isCustomDuration = ref(false);
const customDurationValue = ref<number>();

const handleDurationChange = () => {
    if (selectedDuration.value === 'custom') {
        isCustomDuration.value = true;
        courseInfo.duration = customDurationValue.value ? `${customDurationValue.value}分钟` : '';
    } else {
        isCustomDuration.value = false;
        courseInfo.duration = selectedDuration.value;
    }
};

const handleCustomDurationInput = () => {
    if (customDurationValue.value) {
        courseInfo.duration = `${customDurationValue.value}分钟`;
    }
};

watch(customDurationValue, (newValue) => {
    if (newValue && isCustomDuration.value) {
        courseInfo.duration = `${newValue}分钟`;
    }
});

// 返回上一步
const goBack = () => {
    router.push('/teacher/workbench');
};

// 开始备课
const startPreparation = () => {
    // 这里可以添加表单验证逻辑
    console.log('课程信息：', courseInfo);
    console.log('功能开关：', features);
    console.log('已选标签：', selectedTags.value);

    // 跳转到下一步
    router.push('/teacher/workbench/step2');
};
</script>

<style scoped>
.step1-view {
    height: 100%;
    display: flex;
    gap: 20px;
    max-width: 1400px;
    margin: 0 auto;
    box-sizing: border-box;
}

.main-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 16px;
    height: 100%;
}

.form-section {
    flex: 1;
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    margin-bottom: 16px;
}

.form-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.side-panel {
    width: 300px;
    flex-shrink: 0;
    height: 100%;
}

.features-section {
    height: 100%;
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
}

.features-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.form-item {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.form-row {
    display: flex;
    gap: 20px;
}

.half-width {
    flex: 1;
}

label {
    color: #333;
    font-size: 14px;
}

input,
select,
textarea {
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 14px;
    width: 100%;
}

textarea {
    height: 100px;
    resize: vertical;
}

.feature-row {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.feature-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.feature-label {
    font-size: 14px;
    color: #333;
}

/* 开关样式 */
.toggle-switch {
    position: relative;
    width: 50px;
    height: 24px;
}

.toggle-switch input {
    display: none;
}

.switch {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    transition: .4s;
    border-radius: 24px;
}

.switch:before {
    position: absolute;
    content: "";
    height: 16px;
    width: 16px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    transition: .4s;
    border-radius: 50%;
}

input:checked+.switch {
    background-color: #4c84ff;
}

input:checked+.switch:before {
    transform: translateX(26px);
}

/* 标签样式 */
.special-requirements {
    margin-top: 20px;
}

.special-requirements h3 {
    font-size: 14px;
    color: #333;
    margin-bottom: 10px;
}

.tag-group {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
}

.tag {
    padding: 6px 12px;
    background-color: #f0f2f5;
    border-radius: 4px;
    font-size: 14px;
    color: #666;
    cursor: pointer;
    transition: all 0.3s;
    text-align: center;
}

.tag:hover {
    background-color: #e6e8eb;
}

.tag.active {
    background-color: #4c84ff;
    color: white;
}

/* 按钮样式 */
.actions {
    display: flex;
    justify-content: space-between;
    gap: 16px;
    height: 48px;
}

.action-btn {
    flex: 1;
    height: 100%;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
    padding: 0;
}

.back-btn {
    background-color: #f5f7fa;
    color: #666;
    border: 1px solid #ddd;
}

.back-btn:hover {
    background-color: #e6e8eb;
}

.start-btn {
    background-color: #4c84ff;
    color: white;
}

.start-btn:hover {
    background-color: #3a70e3;
}

/* 课时长度输入样式 */
.duration-input {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.custom-duration {
    display: flex;
    align-items: center;
    gap: 8px;
}

.custom-duration input {
    width: 80px;
    padding: 8px;
}

.unit {
    color: #666;
    font-size: 14px;
}
</style>