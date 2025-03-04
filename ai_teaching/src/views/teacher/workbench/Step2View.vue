<template>
    <div class="step2-view">
        <div class="main-content">
            <div class="content-section">
                <h3>请编辑教案内容：</h3>
                <div class="editor-container">
                    <textarea v-model="lessonContent" placeholder="请在此输入教案内容..."></textarea>
                </div>
            </div>

            <div class="actions">
                <button class="action-btn back-btn" @click="goBack">上一步</button>
                <button class="action-btn save-btn" @click="saveContent">保存</button>
                <button class="action-btn next-btn" @click="nextStep">下一步</button>
            </div>
        </div>

        <!-- 右侧修改建议面板 -->
        <div class="side-panel">
            <div class="suggestions-panel">
                <h3>请输入修改建议：</h3>
                <textarea v-model="modificationSuggestions" placeholder="单击输入文字" :maxlength="200"></textarea>
                <div class="word-count">{{ modificationSuggestions.length }}/200字</div>
                <button class="regenerate-btn" @click="regenerateContent">再次生成</button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const lessonContent = ref('');
const modificationSuggestions = ref('');

// 返回上一步
const goBack = () => {
    router.push('/teacher/workbench/step1');
};

// 保存内容
const saveContent = () => {
    console.log('保存内容', lessonContent.value);
};

// 下一步
const nextStep = () => {
    saveContent();
    router.push('/teacher/workbench/step3');
};

// 重新生成内容
const regenerateContent = () => {
    console.log('根据修改建议重新生成内容');
    // 这里添加重新生成的逻辑
};
</script>

<style scoped>
.step2-view {
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

.content-section {
    flex: 1;
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    margin-bottom: 16px;
}

.editor-container {
    flex: 1;
    display: flex;
    margin-top: 12px;
}

.editor-container textarea {
    width: 100%;
    height: 100%;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    resize: none;
    font-size: 14px;
    line-height: 1.6;
}

.side-panel {
    width: 300px;
    flex-shrink: 0;
    height: 100%;
}

h3 {
    color: #333;
    margin: 0;
    font-size: 16px;
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

/* 右侧修改建议面板样式 */
.suggestions-panel {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    height: 100%;
    display: flex;
    flex-direction: column;
}

.suggestions-panel textarea {
    flex: 1;
    margin: 12px 0 8px;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
    resize: none;
    font-size: 14px;
}

.word-count {
    text-align: right;
    color: #666;
    font-size: 12px;
    margin-bottom: 12px;
}

.regenerate-btn {
    width: 100%;
    height: 48px;
    background-color: #4c84ff;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
    padding: 0;
}

.regenerate-btn:hover {
    background-color: #3a70e3;
}
</style>