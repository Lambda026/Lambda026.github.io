<template>
  <div class="marquee">
    <!-- 逐行渲染：每一行都展示所有 texts 并随机速度滚动 -->
    <div
        class="marquee-row"
        v-for="(duration, rowIndex) in durations"
        :key="rowIndex"
    >
      <!-- 轮播容器，内联样式控制动画时长 -->
      <div
          class="marquee-content"
          :style="{ animationDuration: duration }"
      >
        <!-- 在同一行内，遍历 texts 数组，并重复两遍以实现无缝滚动 -->
        <span
            class="item"
            v-for="(text, idx) in texts"
            :key="idx"
        >
          {{ text }}
          <span class="separator">|</span>
        </span>
        <span
            class="item"
            v-for="(text, idx) in texts"
            :key="'dup-' + idx"
        >
          {{ text }}
          <span class="separator">|</span>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TextMarquee',
  data() {
    return {
      // 需要滚动的所有句子
      texts: [
        "AI自动识别并提取微信聊天中的重要事件",
        "快速整理聊天记录，支持按时间、地点和人物分类",
        "随时随地回顾聊天纪要，帮助你更好地管理信息",
        "强大的搜索功能，一键查询所需事件与细节",
        "数据加密存储，保障你的聊天隐私和安全",
        "与团队共享聊天精华，提升协作效率",
        "自动生成简报，省去手动整理的繁琐",
        "支持多语言聊天记录，跨国沟通无障碍",
        "可视化分析聊天数据，助你洞察趋势与模式",
        "多端同步，任何设备都能查看最新记录"
      ],
      // 你想要的行数
      lineCount: 3,
      // 用于存储每行随机动画时长
      durations: []
    }
  },
  created() {
    // 定义动画时长的随机范围（单位：秒）
    const min = 25
    const max = 40
    // 根据行数，生成若干随机动画时长
    for (let i = 0; i < this.lineCount; i++) {
      const randomDuration = (Math.random() * (max - min) + min).toFixed(2) + 's'
      this.durations.push(randomDuration)
    }
  }
}
</script>

<style scoped>
/* 外层容器：控制整体宽度、背景色、隐藏溢出 */
.marquee {
  position: relative;
  overflow: hidden;
  width: 90%;     /* 根据需要可改成固定宽度，例如 80% */
  background: #222; /* 深色背景，配合浅文字 */
  margin-top:50px;
}

/* 左右两侧渐变消失 */
.marquee::before,
.marquee::after {
  content: "";
  position: absolute;
  top: 0;
  width: 50px;       /* 渐变区域宽度，可根据需要调整 */
  height: 100%;
  z-index: 2;
}
.marquee::before {
  left: 0;
  background: linear-gradient(to right, #222, transparent);
}
.marquee::after {
  right: 0;
  background: linear-gradient(to left, #222, transparent);
}

/* 每一行独立的滚动区域 */
.marquee-row {
  overflow: hidden;
  white-space: nowrap;
  margin: 10px 0; /* 每行之间的间距 */
}

/* 滚动内容容器，通过 animation-duration 实现不同速度 */
.marquee-content {
  display: inline-block;
  animation-name: marquee;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}

/* 文字样式与分隔符 */
.item {
  display: inline-block;
  color: #aaa;        /* 低调的灰色文字 */
  margin-right: 15px; /* 句子之间的间距 */
}
.separator {
  margin: 0 5px;
  color: #B8860B;  /* 较深的黄色 */       /* 分隔符颜色稍深，可根据需求调整 */
}

/* 定义滚动动画：向左移动 50%，完成一次循环 */
@keyframes marquee {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-50%);
  }
}
</style>
