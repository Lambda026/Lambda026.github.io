<!-- MultiColumnWaterfallJournal.vue -->
<template>
  <div class="feature-item">
    <i :class="iconClass" class="icon"></i>
    <h3  style="color:white;">{{ title }}</h3>
    <p style="color:white;">{{ description }}</p>

    <div class="waterfall-container" ref="container">
      <!-- 创建多列瀑布流，每列有不同的速度 -->
      <div
          v-for="(column, columnIndex) in columns"
          :key="'column-' + columnIndex"
          class="waterfall-column"
          :style="{ width: `${100 / columnsCount}%` }"
      >
        <div
            class="waterfall-track"
            :ref="el => { if(el) trackRefs[columnIndex] = el }"
        >
          <div class="waterfall-content" :ref="el => { if(el) contentRefs[columnIndex] = el }">
            <div
                v-for="(image, imageIndex) in columnImages[columnIndex]"
                :key="'original-' + columnIndex + '-' + imageIndex"
                class="waterfall-item"
            >
              <img
                  :src="image.src"
                  :alt="image.alt || '手账示例'"
                  loading="lazy"
                  @load="handleImageLoad(columnIndex)"
              />
            </div>
          </div>
          <!-- 克隆的内容用于无缝滚动 -->
          <div class="waterfall-content clone">
            <div
                v-for="(image, imageIndex) in columnImages[columnIndex]"
                :key="'clone-' + columnIndex + '-' + imageIndex"
                class="waterfall-item"
            >
              <img
                  :src="image.src"
                  :alt="image.alt || '手账示例'"
                  loading="lazy"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MultiColumnWaterfallJournal',

  props: {
    // 标题
    title: {
      type: String,
      default: '每日手账'
    },
    // 描述文字
    description: {
      type: String,
      default: '自动生成每日手账，让您轻松记录生活点滴与心情感悟。精美模板随心切换，定制专属于您的记忆空间。'
    },
    // 基础滚动速度 (像素/秒)
    baseScrollSpeed: {
      type: Number,
      default: 30
    },
    // 列数
    columnsCount: {
      type: Number,
      default: 4
    },
    // 图标类名 (FontAwesome)
    iconClass: {
      type: String,
      default: 'fas fa-book-open'
    }
  },

  data() {
    return {
      isAnimating: false,
      animationId: null,
      lastTimestamp: 0,
      scrollPositions: [],
      contentHeights: [],
      columnImages: [],
      allImages: [],
      columns: [],
      trackRefs: [],
      contentRefs: [],
      imagesLoaded: 0,
      totalImagesToLoad: 0,
      allImagesLoaded: false
    };
  },

  created() {
    // 在组件创建时加载图片
    this.allImages = this.getDailyJournalImages();

    // 初始化列
    this.columns = Array(this.columnsCount).fill().map((_, idx) => ({
      // 每列速度稍有不同，形成交错效果 - 使用反向速度让动画看起来更有变化
      speed: this.baseScrollSpeed * (0.7 + ((this.columnsCount - idx) * 0.15))
    }));

    // 初始化滚动位置
    this.scrollPositions = Array(this.columnsCount).fill(0);

    // 初始化内容高度
    this.contentHeights = Array(this.columnsCount).fill(0);

    // 初始化引用数组
    this.trackRefs = Array(this.columnsCount).fill(null);
    this.contentRefs = Array(this.columnsCount).fill(null);

    // 将图片分配到不同列
    this.distributeImagesToColumns();

    // 计算需要加载的图片总数
    this.totalImagesToLoad = this.allImages.length;
  },

  mounted() {
    this.initWaterfall();

    // 监听窗口大小变化
    window.addEventListener('resize', this.handleResize);

    // 鼠标悬停暂停动画
    this.$refs.container.addEventListener('mouseenter', this.pauseAnimation);
    this.$refs.container.addEventListener('mouseleave', this.resumeAnimation);

    // 设置一个备用的定时器，以防图片加载事件未触发
    setTimeout(() => {
      if (!this.allImagesLoaded) {
        this.allImagesLoaded = true;
        this.measureContentHeights();
        this.startAnimation();
      }
    }, 2000);
  },

  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
    if (this.$refs.container) {
      this.$refs.container.removeEventListener('mouseenter', this.pauseAnimation);
      this.$refs.container.removeEventListener('mouseleave', this.resumeAnimation);
    }
    this.pauseAnimation();
  },

  methods: {
    /**
     * 处理图片加载事件
     */
    handleImageLoad(columnIndex) {
      this.imagesLoaded++;

      // 当所有图片加载完成后，更新高度并开始动画
      if (this.imagesLoaded >= this.totalImagesToLoad && !this.allImagesLoaded) {
        this.allImagesLoaded = true;
        this.$nextTick(() => {
          this.measureContentHeights();
          this.startAnimation();
        });
      }
    },

    /**
     * 将图片均匀分配到不同列
     */
    distributeImagesToColumns() {
      this.columnImages = Array(this.columnsCount).fill().map(() => []);

      // 将图片分配到各列，确保图片均匀分布
      this.allImages.forEach((image, index) => {
        const columnIndex = index % this.columnsCount;
        this.columnImages[columnIndex].push(image);
      });
    },

    initWaterfall() {
      // 在DOM更新后测量内容高度
      this.$nextTick(() => {
        // 如果图片已经加载完成，则直接开始动画
        if (this.allImagesLoaded) {
          this.measureContentHeights();
          this.startAnimation();
        }
      });
    },

    measureContentHeights() {
      // 测量每列内容的高度
      for (let i = 0; i < this.columnsCount; i++) {
        if (this.contentRefs[i]) {
          this.contentHeights[i] = this.contentRefs[i].offsetHeight;

          // 确保有足够的内容高度进行滚动
          if (this.contentHeights[i] < this.$refs.container.offsetHeight) {
            // 如果内容不够，复制当前内容直到足够
            const additionalItems = [...this.columnImages[i]];
            this.columnImages[i] = [...this.columnImages[i], ...additionalItems];

            // 在下一个循环中重新测量
            this.$nextTick(() => {
              if (this.contentRefs[i]) {
                this.contentHeights[i] = this.contentRefs[i].offsetHeight;
              }
            });
          }
        }
      }
    },

    handleResize() {
      // 重新测量内容高度
      this.pauseAnimation();
      this.$nextTick(() => {
        this.measureContentHeights();
        this.resumeAnimation();
      });
    },

    /**
     * 获取每日手账图片数组
     * 使用静态引用公共目录中的图片
     * @returns {Array<{src: string, alt: string}>} 图片数组
     */
    getDailyJournalImages() {
      // 在浏览器环境中，我们不能使用 fs 模块来读取目录
      // 相反，我们必须手动列出我们知道存在的图片
      return [
        { src: '/public/views/dailyJournal/img.png', alt: 'daily Journal' },
        { src: '/public/views/dailyJournal/img_1.png', alt: 'daily Journal' },
        { src: '/public/views/dailyJournal/img_2.png', alt: 'daily Journal' },
        { src: '/public/views/dailyJournal/img_3.png', alt: 'daily Journal' },
        { src: '/public/views/dailyJournal/img_4.png', alt: 'daily Journal' },
        { src: '/public/views/dailyJournal/img_5.png', alt: 'daily Journal' },
        { src: '/public/views/dailyJournal/img_6.png', alt: 'daily Journal' },
      ];
    },

    startAnimation() {
      if (this.animationId) return;

      this.isAnimating = true;
      this.lastTimestamp = performance.now();

      const animate = (timestamp) => {
        if (!this.isAnimating) return;

        const elapsed = timestamp - this.lastTimestamp;
        this.lastTimestamp = timestamp;

        // 更新每列的滚动位置
        for (let i = 0; i < this.columnsCount; i++) {
          // 只有当内容高度大于0时才进行滚动
          if (this.contentHeights[i] > 0) {
            // 根据时间和该列的速度计算滚动量
            const scrollDelta = (elapsed / 1000) * this.columns[i].speed;
            this.scrollPositions[i] = (this.scrollPositions[i] + scrollDelta) % this.contentHeights[i];

            // 应用新位置
            if (this.trackRefs[i]) {
              this.trackRefs[i].style.transform = `translateY(-${this.scrollPositions[i]}px)`;
            }
          }
        }

        this.animationId = requestAnimationFrame(animate);
      };

      this.animationId = requestAnimationFrame(animate);
    },

    pauseAnimation() {
      this.isAnimating = false;
      if (this.animationId) {
        cancelAnimationFrame(this.animationId);
        this.animationId = null;
      }
    },

    resumeAnimation() {
      if (!this.isAnimating) {
        this.startAnimation();
      }
    }
  }
};
</script>

<style scoped>
.feature-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  font-family: 'Arial', sans-serif;
  background-image: linear-gradient(45deg, rgba(255,166,0,1) 14.7%, rgba(255,99,97,1) 73%);
  color: white;
  border-radius: 8px; /* 轻微圆角，符合现代设计 */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.4); /* 加强深色阴影，提升质感 */
}




.icon {
  color: #4a86e8;
  font-size: 2.5rem;
  margin-bottom: 15px;
}

h3 {
  font-size: 1.8rem;
  margin-bottom: 10px;
  color: #333;
}

p {
  font-size: 1.1rem;
  color: #666;
  margin-bottom: 20px;
  line-height: 1.6;
}

.waterfall-container {
  position: relative;
  width: 100%;
  height: 900px;
  margin: 0 auto;
  overflow: hidden;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  display: flex;
}

.waterfall-column {
  position: relative;
  height: 100%;
  overflow: hidden;
}

.waterfall-track {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  will-change: transform;
}

.waterfall-content {
  display: flex;
  flex-direction: column;
}

.waterfall-content.clone {
  position: absolute;
  top: 100%;
  left: 0;
}

.waterfall-item {
  padding: 8px;
  transition: transform 0.3s ease;
}

.waterfall-item:hover {
  transform: scale(1.03);
  z-index: 1;
}

.waterfall-item img {
  width: 100%;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  display: block;
}

@media (max-width: 768px) {
  .waterfall-container {
    height: 400px;
  }
}

@media (max-width: 480px) {
  .waterfall-container {
    height: 350px;
  }
}
</style>