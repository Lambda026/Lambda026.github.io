<template>
  <div style="display: flex;
  flex-direction: column;
  align-items: center;
  width:100%;
  margin: 200px auto 0;
  padding: 20px;
  text-align: center;
  font-family: 'Arial', sans-serif;
  background-image: linear-gradient(45deg, rgba(12, 32, 89, 1) 20%, rgba(67, 97, 238, 1) 50%, rgba(142, 45, 226, 1) 80%);





  color:white;">
    <h3  style="color:white;">聊天事件自动提取</h3>
    <p style="color:white;">无需手动记录事件，自动提取聊天中的重点事件，同时临期进行提醒</p>
  <div class="notification-extractor">
    <!-- 左侧部分 -->
    <div class="left-section">
      <!-- 左上角微信图标与红点通知 -->
      <div class="wechat-corner">
        <div class="wechat-icon">
          <div class="icon-circle">
            <i class="icon-wechat"></i>
          </div>
          <div class="badge">99+</div>
        </div>

      </div>

      <!-- 模糊的消息气泡群组，给人很多消息的感觉 -->
      <div class="message-bubbles">
        <!-- 第一组消息 -->
        <div class="message-group">
          <div class="message-dot primary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
          </div>
        </div>

        <!-- 第二组消息 -->
        <div class="message-group">
          <div class="message-dot secondary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
            <div class="bubble-content short"></div>
          </div>
        </div>

        <!-- 第三组消息 -->
        <div class="message-group">
          <div class="message-dot primary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
          </div>
        </div>

        <!-- 第四组消息 -->
        <div class="message-group">
          <div class="message-dot secondary"></div>
          <div class="message-bubble medium">
            <div class="bubble-header"></div>
            <div class="bubble-content short"></div>
          </div>
        </div>

        <!-- 第五组消息 -->
        <div class="message-group">
          <div class="message-dot primary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
            <div class="bubble-content medium"></div>
          </div>
        </div>
        <!-- 第一组消息 -->
        <div class="message-group">
          <div class="message-dot primary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
          </div>
        </div>

        <!-- 第二组消息 -->
        <div class="message-group">
          <div class="message-dot secondary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
            <div class="bubble-content short"></div>
          </div>
        </div>

        <!-- 第三组消息 -->
        <div class="message-group">
          <div class="message-dot primary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
          </div>
        </div>

        <!-- 第四组消息 -->
        <div class="message-group">
          <div class="message-dot secondary"></div>
          <div class="message-bubble medium">
            <div class="bubble-header"></div>
            <div class="bubble-content short"></div>
          </div>
        </div>

        <!-- 第五组消息 -->
        <div class="message-group">
          <div class="message-dot primary"></div>
          <div class="message-bubble large">
            <div class="bubble-header"></div>
            <div class="bubble-content"></div>
            <div class="bubble-content medium"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- 中间箭头 -->
    <div class="arrow-section">
      <div class="arrow-right">
        <i class="icon-arrow-right"></i>
      </div>
      <p class="arrow-text">内容提取</p>
    </div>

    <!-- 右侧提取内容区域（简化） -->
    <div class="content-section">


    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: 'NotificationExtractor',
  data() {
    return {
      // 组件数据可以在这里定义
    }
  },
  methods: {
    // 组件方法可以在这里定义
  }
}
</script>

<style scoped>
.notification-extractor {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  width:100%;
  height:900px;
  background-color: transparent;
  border-radius: 0.5rem;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

/* 响应式布局 */
@media (max-width: 768px) {
  .notification-extractor {
    flex-direction: column;
  }

  .arrow-section {
    transform: rotate(90deg);
    margin: 2rem 0;
  }
}

/* 左侧部分 */
.left-section {
  position: relative;
  height: 750px;
  width: 600px;
  background-color: #f0f0f0;
  border-radius: 1rem;
  padding: 1rem;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

/* 左上角微信图标区域 */
.wechat-corner {
  position: absolute;
  top: 1rem;
  left: 1rem;
  display: flex;
  align-items: flex-start;
}

.wechat-icon {
  position: relative;
  margin-right: 0.5rem;
}

.icon-circle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2.8rem;
  height: 2.8rem;
  border-radius: 50%;
}

.icon-wechat::before {
  content: "";
  display: inline-block;
  width: 1.8rem;
  height: 1.8rem;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}

.badge {
  position: absolute;
  top: -0.25rem;
  right: -0.25rem;
  min-width: 1.8rem;
  height: 1.8rem;
  padding: 0 0.4rem;
  background-color: #FF3B30;
  color: white;
  font-size: 0.75rem;
  font-weight: bold;
  border-radius: 0.9rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 消息点状态指示器 */
.message-dot {
  min-width: 0.75rem;
  height: 0.75rem;
  border-radius: 50%;
  margin-right: 0.5rem;
  margin-top: 0.75rem;
}

.message-dot.primary {
  background-color: #FF3B30;
}

.message-dot.secondary {
  background-color: #FF9500;
}

/* 模糊的消息气泡群组 */
.message-bubbles {
  position: absolute;
  top: 4.5rem;
  bottom: 1rem;
  left: 1rem;
  right: 1rem;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  overflow-y: auto;
}

.message-group {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
}

.message-bubble {
  background-color: white;
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  padding: 0.5rem;
  flex-grow: 1;
}

.message-bubble.large {
  min-height: 3.5rem;
}

.message-bubble.medium {
  min-height: 2.5rem;
}

.bubble-header {
  height: 0.75rem;
  width: 40%;
  background-color: #e0e0e0;
  border-radius: 0.25rem;
  margin-bottom: 0.5rem;
}

.bubble-content {
  height: 0.65rem;
  width: 100%;
  background-color: #f0f0f0;
  border-radius: 0.25rem;
  margin-bottom: 0.5rem;
}

.bubble-content.short {
  width: 60%;
}

.bubble-content.medium {
  width: 80%;
}

/* 中间箭头部分 */
.arrow-section {
  margin: 0 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.arrow-right {
  display: flex;
  justify-content: center;
  align-items: center;
}

.icon-arrow-right::before {
  content: "";
  display: inline-block;
  width: 3rem;
  height: 3rem;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' fill='%231989FA' viewBox='0 0 24 24'%3E%3Cpath d='M12 4l-1.41 1.41L16.17 11H4v2h12.17l-5.58 5.59L12 20l8-8z'%3E%3C/path%3E%3C/svg%3E");
  background-size: contain;
  background-repeat: no-repeat;
}

.arrow-text {
  margin-top: 0.5rem;
  font-size: 0.75rem;
  color: #999;
  text-align: center;
}

/* 右侧内容提取部分 */
.content-section {
  border-radius: 0.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  width: 1100px;
  height: 750px;
  background-color: white;
  background-image:url("public/views/Notifications/Notification.png");
  background-size: cover;
}

.content-header {
  border-bottom: 1px solid #eee;
  padding-bottom: 0.75rem;
  margin-bottom: 1.5rem;
}

.content-header h3 {
  margin: 0;
  font-size: 1.125rem;
  font-weight: 600;
  color: #333;
}


.content-placeholder {
  background-color: #f7f7f7;
  padding: 1rem;
  border-radius: 0.5rem;
}

.placeholder-line {
  height: 0.75rem;
  width: 100%;
  background-color: #e0e0e0;
  border-radius: 0.375rem;
  margin-bottom: 0.75rem;
}

.placeholder-line.short {
  width: 60%;
}

.placeholder-line.medium {
  width: 80%;
}

.action-button {
  margin-top: 2rem;
  text-align: center;
}

.action-button button {
  background-color: #1989FA;
  color: white;
  border: none;
  border-radius: 0.375rem;
  padding: 0.5rem 1.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.action-button button:hover {
  background-color: #0076E4;
}
</style>