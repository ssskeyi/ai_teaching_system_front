<template>
    <div class="step4-view">
        <div class="main-content">
            <div class="question-settings" :class="{ collapsed: isSettingsCollapsed }">
                <div class="settings-header">
                    <h3>题目设置</h3>
                    <button class="collapse-btn" @click="toggleSettings">
                        {{ isSettingsCollapsed ? '展开' : '收起' }}
                    </button>
                </div>
                <div class="settings-content" v-show="!isSettingsCollapsed">
                    <div class="question-types">
                        <h3>题型选择：</h3>
                        <div class="type-buttons">
                            <div class="type-button" :class="{ active: selectedTypes.includes('choice') }"
                                @click="toggleType('choice')">
                                选择题
                                <div class="count-input" @click.stop>
                                    <input type="number" v-model="questionCounts.choice" min="0" max="99"
                                        @input="validateCount('choice')">
                                    <span class="unit">道</span>
                                </div>
                            </div>
                            <div class="type-button" :class="{ active: selectedTypes.includes('judgment') }"
                                @click="toggleType('judgment')">
                                判断题
                                <div class="count-input" @click.stop>
                                    <input type="number" v-model="questionCounts.judgment" min="0" max="99"
                                        @input="validateCount('judgment')">
                                    <span class="unit">道</span>
                                </div>
                            </div>
                            <div class="type-button" :class="{ active: selectedTypes.includes('blank') }"
                                @click="toggleType('blank')">
                                填空题
                                <div class="count-input" @click.stop>
                                    <input type="number" v-model="questionCounts.blank" min="0" max="99"
                                        @input="validateCount('blank')">
                                    <span class="unit">道</span>
                                </div>
                            </div>
                            <div class="type-button" :class="{ active: selectedTypes.includes('short') }"
                                @click="toggleType('short')">
                                简答题
                                <div class="count-input" @click.stop>
                                    <input type="number" v-model="questionCounts.short" min="0" max="99"
                                        @input="validateCount('short')">
                                    <span class="unit">道</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="difficulty-setting">
                        <h3>难度设置：</h3>
                        <div class="difficulty-slider-container">
                            <div class="difficulty-labels">
                                <span>易</span>
                                <span>较易</span>
                                <span>中</span>
                                <span>较难</span>
                                <span>难</span>
                            </div>
                            <div class="slider-wrapper">
                                <input type="range" v-model="difficulty" :min="1" :max="5" :step="1"
                                    class="difficulty-slider">
                            </div>
                        </div>
                    </div>

                    <button class="generate-btn" @click="generateQuestions">一键生成</button>
                </div>
            </div>

            <div class="questions-list">
                <h3>题目列表：</h3>
                <div class="questions-container">
                    <div v-for="(question, index) in questions" :key="index" class="question-item"
                        :class="{ selected: selectedQuestions.includes(index) }"
                        @click="toggleQuestionSelection(index)">
                        <div class="question-header">
                            <span class="question-number">题目 {{ index + 1 }}</span>
                            <button class="delete-btn" @click.stop="deleteQuestion(index)">删除</button>
                        </div>
                        <div class="question-content">{{ question.content }}</div>
                        <div class="question-type">{{ question.type }}</div>
                    </div>
                </div>
                <button class="add-question-btn" @click="addQuestion">添加题目</button>
            </div>

            <div class="actions">
                <button class="action-btn back-btn" @click="goBack">上一步</button>
                <button class="action-btn save-btn" @click="saveContent">保存</button>
                <button class="action-btn next-btn" @click="nextStep">完成</button>
            </div>
        </div>

        <div class="side-panel">
            <div class="suggestion-input">
                <h3>修改建议：</h3>
                <textarea v-model="suggestion" placeholder="请输入修改建议..." maxlength="200"></textarea>
                <div class="word-count">{{ suggestion.length }}/200字</div>
                <button class="regenerate-btn" @click="regenerateQuestions">重新生成</button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// 选中的题型
const selectedTypes = ref<string[]>([]);

// 题目数量统计
const questionCounts = reactive({
    choice: 0,
    judgment: 0,
    blank: 0,
    short: 0
});

// 难度设置
const difficulty = ref(3);

// 难度等级映射
const difficultyLevels: Record<string, string> = {
    '1': '易',
    '2': '较易',
    '3': '中',
    '4': '较难',
    '5': '难'
};

// 题目列表
const questions = ref([
    { content: '这是一道选择题示例...', type: '选择题' },
    { content: '这是一道判断题示例...', type: '判断题' }
]);

// 选中的题目
const selectedQuestions = ref<number[]>([]);

// 修改建议
const suggestion = ref('');

// 验证题目数量
const validateCount = (type: keyof typeof questionCounts) => {
    let value = questionCounts[type];
    if (value < 0) questionCounts[type] = 0;
    if (value > 99) questionCounts[type] = 99;
};

// 获取难度文字说明
const getDifficultyText = (value: number) => {
    if (value <= 20) return '易';
    if (value <= 40) return '较易';
    if (value <= 60) return '中';
    if (value <= 80) return '较难';
    return '难';
};

// 切换题型选择
const toggleType = (type: string) => {
    const index = selectedTypes.value.indexOf(type);
    if (index === -1) {
        selectedTypes.value.push(type);
    } else {
        selectedTypes.value.splice(index, 1);
    }
};

// 切换题目选中状态
const toggleQuestionSelection = (index: number) => {
    const selectedIndex = selectedQuestions.value.indexOf(index);
    if (selectedIndex === -1) {
        selectedQuestions.value.push(index);
    } else {
        selectedQuestions.value.splice(selectedIndex, 1);
    }
};

// 删除题目
const deleteQuestion = (index: number) => {
    questions.value.splice(index, 1);
};

// 添加题目
const addQuestion = () => {
    questions.value.push({
        content: '新题目内容...',
        type: '选择题'
    });
};

// 重新生成题目
const regenerateQuestions = () => {
    console.log('根据修改建议重新生成题目');
};

// 返回上一步
const goBack = () => {
    const features = JSON.parse(localStorage.getItem('workbenchFeatures') || '{}');
    if (features.resourceRecommend) {
        router.push('/admin/workbench/step3');
    } else {
        router.push('/admin/workbench/step1');
    }
};

// 保存内容
const saveContent = () => {
    console.log('保存题目内容', {
        selectedTypes: selectedTypes.value,
        difficulty: difficulty.value,
        questions: questions.value,
        suggestion: suggestion.value
    });
};

// 完成
const nextStep = () => {
    saveContent();
    // 清除功能选择记录
    localStorage.removeItem('workbenchFeatures');
    // 返回工作台首页
    router.push('/admin/workbench');
};

// 更新难度文字说明
const updateDifficultyText = () => {
    return difficultyLevels[difficulty.value.toString()];
};

const isSettingsCollapsed = ref(false);

const toggleSettings = () => {
    isSettingsCollapsed.value = !isSettingsCollapsed.value;
};

const generateQuestions = () => {
    console.log('一键生成题目', {
        selectedTypes: selectedTypes.value,
        questionCounts,
        difficulty: difficulty.value
    });
};
</script>

<style scoped>
.step4-view {
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
    height: 100%;
    position: relative;
    padding-bottom: 64px;
    /* 为底部按钮预留空间 */
}

.side-panel {
    width: 300px;
    height: 100%;
}

.question-settings {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    margin-bottom: 16px;
}

.question-settings.collapsed {
    padding: 12px 16px;
}

.settings-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 16px;
}

.collapse-btn {
    padding: 4px 12px;
    background: #f5f7fa;
    border: 1px solid #ddd;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
    font-size: 14px;
    transition: all 0.3s;
}

.collapse-btn:hover {
    background: #e6e8eb;
}

.settings-content {
    transition: all 0.3s ease;
}

h3 {
    margin: 0 0 12px 0;
    font-size: 16px;
    color: #333;
}

.type-buttons,
.difficulty-buttons {
    display: flex;
    gap: 12px;
    flex-wrap: wrap;
}

.type-button,
.difficulty-button {
    padding: 8px 16px;
    border: 1px solid #ddd;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s;
    background: white;
    color: #666;
}

.type-button .count {
    color: #999;
    font-size: 14px;
}

.type-button.active,
.difficulty-button.active {
    background: #4c84ff;
    color: white;
    border-color: #4c84ff;
}

.type-button.active .count {
    color: rgba(255, 255, 255, 0.8);
}

.difficulty-setting {
    margin-top: 16px;
}

.questions-list {
    flex: 1;
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.questions-container {
    flex: 1;
    overflow-y: auto;
    margin-bottom: 16px;
}

.question-item {
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-bottom: 8px;
    cursor: pointer;
    transition: all 0.3s;
}

.question-item.selected {
    border-color: #4c84ff;
    background: rgba(76, 132, 255, 0.05);
}

.question-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 8px;
}

.delete-btn {
    padding: 4px 8px;
    background: #f5f7fa;
    border: none;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
}

.delete-btn:hover {
    background: #e6e8eb;
}

.question-type {
    font-size: 12px;
    color: #666;
    margin-top: 8px;
}

.add-question-btn {
    width: 100%;
    padding: 12px;
    background: #f5f7fa;
    border: 1px dashed #ddd;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
    transition: all 0.3s;
}

.add-question-btn:hover {
    background: #e6e8eb;
}

.actions {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    gap: 16px;
    height: 48px;
    background: transparent;
}

.action-btn {
    flex: 1;
    height: 100%;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
}

.back-btn {
    background-color: #f5f7fa;
    color: #666;
    border: 1px solid #ddd;
}

.back-btn:hover {
    background-color: #e6e8eb;
}

.save-btn {
    background-color: #f5f7fa;
    color: #666;
    border: 1px solid #ddd;
}

.save-btn:hover {
    background-color: #e6e8eb;
}

.next-btn {
    background-color: #4c84ff;
    color: white;
}

.next-btn:hover {
    background-color: #3a70e3;
}

.type-button {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 8px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s;
    background: white;
    color: #666;
    min-width: 160px;
}

.count-input {
    display: flex;
    align-items: center;
    margin-left: 8px;
    background: rgba(0, 0, 0, 0.05);
    padding: 2px 4px;
    border-radius: 4px;
}

.count-input input {
    width: 40px;
    padding: 2px 4px;
    border: 1px solid #ddd;
    border-radius: 2px;
    text-align: center;
    font-size: 14px;
}

.count-input .unit {
    margin-left: 2px;
    font-size: 12px;
    color: inherit;
}

.type-button.active .count-input {
    background: rgba(255, 255, 255, 0.2);
}

.type-button.active .count-input input {
    border-color: rgba(255, 255, 255, 0.3);
    background: rgba(255, 255, 255, 0.1);
    color: white;
}

.difficulty-slider-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
    padding: 0 16px;
}

.difficulty-labels {
    display: flex;
    justify-content: space-between;
    padding: 0 8px;
    color: #666;
    font-size: 14px;
}

.slider-wrapper {
    position: relative;
    padding: 10px 0;
}

.difficulty-slider {
    width: 100%;
    height: 4px;
    background: #e6e8eb;
    border-radius: 2px;
    outline: none;
    -webkit-appearance: none;
    margin: 0;
    cursor: pointer;
}

.difficulty-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 20px;
    height: 20px;
    background: #4c84ff;
    border: 2px solid #fff;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.3s;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    position: relative;
    z-index: 2;
}

.difficulty-slider::-webkit-slider-thumb:hover {
    transform: scale(1.2);
    box-shadow: 0 2px 6px rgba(76, 132, 255, 0.4);
}

.suggestion-input {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    height: 100%;
    display: flex;
    flex-direction: column;
}

.suggestion-input textarea {
    flex: 1;
    min-height: 200px;
    margin-bottom: 12px;
}

.word-count {
    text-align: right;
    color: #666;
    font-size: 12px;
    margin-bottom: 12px;
}

.regenerate-btn {
    width: 100%;
    padding: 12px;
    background: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.3s;
}

.regenerate-btn:hover {
    background: #3a70e3;
}

.generate-btn {
    width: 100%;
    padding: 12px;
    margin-top: 16px;
    background: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
}

.generate-btn:hover {
    background: #3a70e3;
}
</style>