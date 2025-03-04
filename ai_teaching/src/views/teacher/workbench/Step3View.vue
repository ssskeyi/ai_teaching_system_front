<template>
    <div class="step3-view">
        <div class="main-content">
            <h2>资源推荐</h2>

            <!-- 课件部分 -->
            <div class="resource-section">
                <h3>课件</h3>
                <div class="resource-list">
                    <div class="resource-item" :class="{ selected: selectedResources.courseware.includes('1') }"
                        @click="toggleSelection('courseware', '1')">
                        <div class="match-rate">99%</div>
                        <div class="thumbnail">缩略图一</div>
                        <div v-if="selectedResources.courseware.includes('1')" class="check-icon">✓</div>
                        <button class="preview-btn" @click.stop>预览</button>
                    </div>
                    <div class="resource-item" :class="{ selected: selectedResources.courseware.includes('2') }"
                        @click="toggleSelection('courseware', '2')">
                        <div class="match-rate">95%</div>
                        <div class="thumbnail">缩略图二</div>
                        <div v-if="selectedResources.courseware.includes('2')" class="check-icon">✓</div>
                        <button class="preview-btn" @click.stop>预览</button>
                    </div>
                </div>
            </div>

            <!-- 视频部分 -->
            <div class="resource-section">
                <h3>视频</h3>
                <div class="resource-list">
                    <div class="resource-item" :class="{ selected: selectedResources.video.includes('1') }"
                        @click="toggleSelection('video', '1')">
                        <div class="match-rate">97%</div>
                        <div class="thumbnail">缩略图一</div>
                        <div v-if="selectedResources.video.includes('1')" class="check-icon">✓</div>
                        <button class="preview-btn" @click.stop>预览</button>
                    </div>
                    <div class="resource-item" :class="{ selected: selectedResources.video.includes('2') }"
                        @click="toggleSelection('video', '2')">
                        <div class="match-rate">93%</div>
                        <div class="thumbnail">缩略图二</div>
                        <div v-if="selectedResources.video.includes('2')" class="check-icon">✓</div>
                        <button class="preview-btn" @click.stop>预览</button>
                    </div>
                </div>
            </div>

            <!-- 相关图片 -->
            <div class="resource-section">
                <h3>相关图片</h3>
                <div class="resource-list">
                    <div class="resource-item" :class="{ selected: selectedResources.image.includes('1') }"
                        @click="toggleSelection('image', '1')">
                        <div class="match-rate">90%</div>
                        <div class="thumbnail">缩略图一</div>
                        <div v-if="selectedResources.image.includes('1')" class="check-icon">✓</div>
                        <button class="preview-btn" @click.stop>预览</button>
                    </div>
                    <div class="resource-item" :class="{ selected: selectedResources.image.includes('2') }"
                        @click="toggleSelection('image', '2')">
                        <div class="match-rate">85%</div>
                        <div class="thumbnail">缩略图二</div>
                        <div v-if="selectedResources.image.includes('2')" class="check-icon">✓</div>
                        <button class="preview-btn" @click.stop>预览</button>
                    </div>
                </div>
            </div>

            <!-- 底部按钮 -->
            <div class="actions">
                <button class="action-btn back-btn" @click="goBack">上一步</button>
                <button class="action-btn save-btn" @click="saveContent">保存</button>
                <button class="action-btn next-btn" @click="nextStep">下一步</button>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router';
import { reactive } from 'vue';

const router = useRouter();

interface SelectedResources {
    courseware: string[];
    video: string[];
    image: string[];
}

const selectedResources = reactive<SelectedResources>({
    courseware: [],
    video: [],
    image: []
});

const toggleSelection = (type: keyof SelectedResources, id: string) => {
    const index = selectedResources[type].indexOf(id);
    if (index === -1) {
        selectedResources[type].push(id);
    } else {
        selectedResources[type].splice(index, 1);
    }
};

const goBack = () => {
    router.push('/teacher/workbench/step2');
};

const saveContent = () => {
    console.log('保存选择的资源', selectedResources);
};

const nextStep = () => {
    saveContent();
    router.push('/teacher/workbench/step4');
};
</script>

<style scoped>
.step3-view {
    height: 100%;
    display: flex;
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
    padding: 0;
    box-sizing: border-box;
}

h2 {
    margin: 0;
    font-size: 20px;
    color: #333;
}

.resource-section {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h3 {
    margin: 0 0 16px 0;
    font-size: 16px;
    color: #333;
}

.resource-list {
    display: flex;
    gap: 16px;
    overflow-x: auto;
    padding: 4px;
}

.resource-item {
    position: relative;
    width: 200px;
    height: 150px;
    background: #f5f7fa;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    cursor: pointer;
    border: 2px solid transparent;
    transition: all 0.3s ease;
}

.resource-item.selected {
    border-color: #4c84ff;
    background: #f5f7fa;
}

.resource-item:hover {
    border-color: #4c84ff;
    opacity: 0.9;
}

.match-rate {
    position: absolute;
    top: 8px;
    left: 8px;
    background: rgba(76, 132, 255, 0.9);
    color: white;
    padding: 2px 6px;
    border-radius: 4px;
    font-size: 14px;
}

.check-icon {
    position: absolute;
    top: 8px;
    right: 8px;
    color: #4c84ff;
    font-size: 16px;
    font-weight: bold;
    background: white;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.thumbnail {
    color: #666;
    font-size: 14px;
}

.preview-btn {
    position: absolute;
    bottom: 8px;
    right: 8px;
    background: #e6e8eb;
    border: none;
    padding: 4px 8px;
    border-radius: 4px;
    color: #666;
    cursor: pointer;
    font-size: 12px;
}

.preview-btn:hover {
    background: #d8d8d8;
}

/* 按钮样式 */
.actions {
    display: flex;
    justify-content: space-between;
    gap: 16px;
    height: 48px;
    margin-top: auto;
    padding: 0;
    width: 100%;
    box-sizing: border-box;
}

.action-btn {
    flex: 1;
    height: 48px;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
    padding: 0;
    box-sizing: border-box;
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
</style>