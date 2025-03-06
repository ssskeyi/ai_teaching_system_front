<template>
    <div class="class-detail">
        <!-- 顶部班级信息 -->
        <div class="class-header">
            <div class="class-info">
                <h2>软件2202班</h2>
                <span class="info-item">学生人数: 45</span>
                <span class="info-item">班主任: 林子皓</span>
            </div>
            <button class="exit-btn">退出班级</button>
        </div>

        <!-- 主要内容区域 -->
        <div class="main-content">
            <div class="left-content">
                <!-- 班级科目 -->
                <div class="section subject-section">
                    <h3>班级科目</h3>
                    <div class="subject-list">
                        <div class="subject-item">
                            <span class="subject-name">数据结构</span>
                            <span class="status in-progress">进行中</span>
                        </div>
                        <div class="subject-item">
                            <span class="subject-name">嵌入式开发</span>
                            <span class="status in-progress">进行中</span>
                        </div>
                        <div class="subject-item">
                            <span class="subject-name">大学物理（上）</span>
                            <span class="status finished">已结束</span>
                        </div>
                        <div class="subject-item">
                            <span class="subject-name">大学生心理健康教育</span>
                            <span class="status finished">已结束</span>
                        </div>
                        <div class="subject-item">
                            <span class="subject-name">大学生英语写作</span>
                            <span class="status finished">已结束</span>
                        </div>
                    </div>
                </div>

                <!-- 课程资源 -->
                <div class="section resource-section">
                    <h3>课程资源</h3>
                    <div class="resource-tabs">
                        <button class="tab-btn active">图集</button>
                        <button class="tab-btn">课件</button>
                        <button class="tab-btn">视频</button>
                        <button class="tab-btn">文献</button>
                    </div>
                    <div class="resource-list">
                        <div class="resource-item">
                            <span class="resource-name">关于......的文献研究</span>
                            <span class="resource-date">2023/9/27上传</span>
                            <div class="resource-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn">保存</button>
                            </div>
                        </div>
                        <div class="resource-item">
                            <span class="resource-name">高等数学第二章PPT</span>
                            <span class="resource-date">2023/9/10上传</span>
                            <div class="resource-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn">保存</button>
                            </div>
                        </div>
                        <div class="resource-item">
                            <span class="resource-name">古诗词鉴赏专题</span>
                            <span class="resource-date">2023/5/10上传</span>
                            <div class="resource-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn">保存</button>
                            </div>
                        </div>
                        <div class="resource-item">
                            <span class="resource-name">积分中值定理详解</span>
                            <span class="resource-date">2023/1/2上传</span>
                            <div class="resource-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn">保存</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 作业管理 -->
                <div class="section homework-section">
                    <h3>作业管理</h3>
                    <div class="homework-list">
                        <div class="homework-item">
                            <div class="homework-info">
                                <span class="homework-name">第一学期第五次测试</span>
                                <span class="status in-progress">进行中</span>
                            </div>
                            <span class="completion-rate">30/45完成</span>
                            <div class="homework-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn">提交</button>
                            </div>
                        </div>
                        <div class="homework-item">
                            <div class="homework-info">
                                <span class="homework-name">第一学期第四次测试</span>
                                <span class="status in-progress">进行中</span>
                            </div>
                            <span class="completion-rate">44/45完成</span>
                            <div class="homework-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn">提交</button>
                            </div>
                        </div>
                        <div class="homework-item">
                            <div class="homework-info">
                                <span class="homework-name">期中测试</span>
                                <span class="status finished">已结束</span>
                            </div>
                            <span class="completion-rate">45/45完成</span>
                            <div class="homework-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn disabled">提交</button>
                            </div>
                        </div>
                        <div class="homework-item">
                            <div class="homework-info">
                                <span class="homework-name">第一学期第二次测试</span>
                                <span class="status finished">已结束</span>
                            </div>
                            <span class="completion-rate">40/45完成</span>
                            <div class="homework-actions">
                                <button class="action-btn">查看</button>
                                <button class="action-btn disabled">提交</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- 学情分析 -->
                <div class="section analysis-section">
                    <h3>学情分析</h3>
                    <div class="analysis-content">
                        <div class="personal-stats">
                            <div class="stat-item">
                                <span class="stat-label">个人排名：</span>
                                <span class="stat-value">2/45</span>
                            </div>
                            <div class="stat-item">
                                <span class="stat-label">知识点掌握程度：</span>
                                <span class="stat-value">85%</span>
                            </div>
                            <div class="stat-item">
                                <span class="stat-label">学习效果预测：</span>
                                <span class="stat-value"></span>
                            </div>
                            <div class="stat-item">
                                <span class="stat-label">综合评价：</span>
                                <span class="stat-value"></span>
                            </div>
                            <div class="stat-item">
                                <span class="stat-label">学习建议：</span>
                                <span class="stat-value"></span>
                            </div>
                        </div>
                        <div class="analysis-charts">
                            <div class="chart subject-performance">
                                <!-- 各科成绩水平图表 -->
                            </div>
                            <div class="chart study-time">
                                <!-- 各科学习时间占比图表 -->
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 右侧成员列表 -->
            <div class="member-list">
                <div class="member-header">
                    <h3>成员列表</h3>
                    <div class="search-box">
                        <input type="text" placeholder="搜索" />
                        <button class="search-btn">搜索</button>
                    </div>
                </div>
                <div class="member-items">
                    <div class="member-item">
                        <div class="avatar">头像</div>
                        <span class="member-name">林雨婷</span>
                    </div>
                    <div class="member-item">
                        <div class="avatar">头像</div>
                        <span class="member-name">赵亮</span>
                    </div>
                    <!-- 更多成员... -->
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
// 组件逻辑
</script>

<style scoped>
.class-detail {
    padding: 20px;
    height: 100%;
}

.class-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}

.class-info {
    display: flex;
    align-items: center;
    gap: 20px;
}

.info-item {
    color: #666;
}

.exit-btn {
    padding: 8px 20px;
    background-color: #dc3545;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.main-content {
    display: flex;
    gap: 20px;
    height: calc(100% - 60px);
}

.left-content {
    flex: 1;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.section {
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* 班级科目样式 */
.subject-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.subject-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px;
    background: #f5f7fa;
    border-radius: 4px;
}

.status {
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 12px;
}

.status.in-progress {
    background-color: #4c84ff;
    color: white;
}

.status.finished {
    background-color: #999;
    color: white;
}

/* 课程资源样式 */
.resource-tabs {
    display: flex;
    gap: 10px;
    margin-bottom: 16px;
}

.tab-btn {
    padding: 6px 16px;
    border: none;
    border-radius: 4px;
    background: #f5f7fa;
    cursor: pointer;
}

.tab-btn.active {
    background: #4c84ff;
    color: white;
}

.resource-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.resource-item {
    display: grid;
    grid-template-columns: 1fr auto auto;
    align-items: center;
    padding: 12px;
    background: #f5f7fa;
    border-radius: 4px;
    gap: 16px;
}

.resource-name {
    font-size: 14px;
    color: #333;
}

.resource-date {
    color: #666;
    font-size: 14px;
    text-align: right;
}

.resource-actions {
    display: flex;
    gap: 8px;
    min-width: 140px;
    justify-content: flex-end;
}

.action-btn {
    padding: 4px 12px;
    border: none;
    border-radius: 4px;
    background: #4c84ff;
    color: white;
    cursor: pointer;
}

.action-btn.disabled {
    background: #999;
    cursor: not-allowed;
}

/* 作业管理样式 */
.homework-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.homework-item {
    display: grid;
    grid-template-columns: 1fr auto auto;
    align-items: center;
    padding: 12px;
    background: #f5f7fa;
    border-radius: 4px;
    gap: 16px;
}

.homework-info {
    display: flex;
    align-items: center;
    gap: 12px;
}

.completion-rate {
    color: #666;
    font-size: 14px;
    text-align: right;
    min-width: 80px;
}

.homework-actions {
    display: flex;
    gap: 8px;
    min-width: 140px;
    justify-content: flex-end;
}

/* 学情分析样式 */
.analysis-content {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.personal-stats {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

.stat-item {
    display: flex;
    gap: 8px;
}

.analysis-charts {
    display: flex;
    gap: 20px;
    height: 200px;
}

.chart {
    flex: 1;
    background: #f5f7fa;
    border-radius: 4px;
}

/* 成员列表样式 */
.member-list {
    width: 300px;
    background: white;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.member-header {
    margin-bottom: 16px;
}

.search-box {
    display: flex;
    gap: 8px;
    margin-top: 12px;
}

.search-box input {
    flex: 1;
    padding: 6px 12px;
    border: 1px solid #ddd;
    border-radius: 4px;
}

.member-items {
    display: flex;
    flex-direction: column;
    gap: 12px;
    overflow-y: auto;
    max-height: calc(100% - 80px);
}

.member-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 8px;
    background: #f5f7fa;
    border-radius: 4px;
}

.avatar {
    width: 40px;
    height: 40px;
    background: #ddd;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 12px;
}
</style>