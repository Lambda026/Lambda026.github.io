<script setup>
import {onMounted, onUnmounted, ref} from 'vue';
import gsap from 'gsap';
import TextCarousel from "@/components/TextCarousel.vue";
import OptimizedWaterfallJournal from "@/components/OptimizedWaterfallJournal.vue";
import NotificationExtractor from "@/components/Notification.vue";

const text = ref('Wecord');
const finalText = 'We Record';
const isAnimating = ref(false);
const letterSpacing = ref('0px');
const delayBetweenLoops = 3000;
const startText = 'Wecord'; // 初始短文本
onMounted(() => {
  if (window) {
    window.scrollTo({
      top: 0,
      left: 0,
      behavior: 'instant'
    });
  }
  startAnimationLoop();
  // document.documentElement.style.overflow = 'hidden';

  const canvas = document.getElementById('c');
  if (!canvas) return;
  let w = canvas.width = window.innerWidth,
      h = canvas.height = window.innerHeight,
      ctx = canvas.getContext('2d');

  const opts = {
    lineCount: 100,
    starCount: 30,
    radVel: 0.01,
    lineBaseVel: 0.1,
    lineAddedVel: 0.1,
    lineBaseLife: 0.4,
    lineAddedLife: 0.01,
    starBaseLife: 10,
    starAddedLife: 10,
    ellipseTilt: -0.3,
    ellipseBaseRadius: 0.15,
    ellipseAddedRadius: 0.02,
    ellipseAxisMultiplierX: 2,
    ellipseAxisMultiplierY: 1,
    ellipseCX: w / 2,
    ellipseCY: h / 2,
    repaintAlpha: 0.015
  };

  let gui;
  if (typeof dat !== 'undefined') {
    gui = new dat.GUI();
    let f = gui.addFolder('logics');
    f.add(opts, 'lineCount', 1, 300);
    f.add(opts, 'starCount', 1, 300);
    f.add(opts, 'radVel', 0, 1);
    f.add(opts, 'lineBaseVel', 0.01, 1);
    f.add(opts, 'lineAddedVel', 0, 1);
    f.add(opts, 'lineBaseLife', 0, 1);
    f.add(opts, 'lineAddedLife', 0, 1);
    f.add(opts, 'starBaseLife', 0, 100);
    f.add(opts, 'starAddedLife', 0, 100);
    f = gui.addFolder('graphics');
    f.add(opts, 'ellipseTilt', -Math.PI, Math.PI).step(0.1);
    f.add(opts, 'ellipseBaseRadius', 0, 0.5);
    f.add(opts, 'ellipseAddedRadius', 0, 0.5);
    f.add(opts, 'ellipseAxisMultiplierX', 0, 3);
    f.add(opts, 'ellipseAxisMultiplierY', 0, 3);
    f.add(opts, 'ellipseCX', 0, w);
    f.add(opts, 'ellipseCY', 0, h);
    f.add(opts, 'repaintAlpha', 0, 1);
    gui.add(window, 'init').name('reset animation');
    gui.add(window, 'LuukLamers');
  }

  let lines = [],
      stars = [],
      tick = 0,
      first = true;

  function init() {
    lines.length = stars.length = 0;
    ctx.globalCompositeOperation = 'source-over';
    ctx.fillStyle = '#333';
    ctx.fillRect(0, 0, w, h);
    if (first && gui) {
      loop();
      first = false;
    } else {
      loop();
    }
  }

  function loop() {
    window.requestAnimationFrame(loop);
    step();
    draw();
  }

  function step() {
    tick += 0.5;
    if (lines.length < opts.lineCount && Math.random() < 0.5)
      lines.push(new Line());
    if (stars.length < opts.starCount)
      stars.push(new Star());
    lines.forEach(line => line.step());
    stars.forEach(star => star.step());
  }

  function draw() {
    ctx.shadowBlur = 0;
    ctx.globalCompositeOperation = 'source-over';
    ctx.fillStyle = 'rgba(0,0,0,' + opts.repaintAlpha + ')';
    ctx.fillRect(0, 0, w, h);
    ctx.globalCompositeOperation = 'lighter';
    ctx.save();
    ctx.translate(opts.ellipseCX, opts.ellipseCY);
    ctx.rotate(opts.ellipseTilt);
    ctx.scale(opts.ellipseAxisMultiplierX, opts.ellipseAxisMultiplierY);
    lines.forEach(line => line.draw());
    ctx.restore();
    stars.forEach(star => star.draw());
  }

  function Line() {
    this.reset();
  }
  Line.prototype.reset = function() {
    this.rad = Math.random() * Math.PI * 2;
    this.len = w * (opts.ellipseBaseRadius + Math.random() * opts.ellipseAddedRadius);
    this.lenVel = opts.lineBaseVel + Math.random() * opts.lineAddedVel;
    this.x = this.px = Math.cos(this.rad) * this.len;
    this.y = this.py = Math.sin(this.rad) * this.len;
    this.life = this.originalLife = w * (opts.lineBaseLife + Math.random() * opts.lineAddedLife);
    this.alpha = 0.2 + Math.random() * 0.8;
  };
  Line.prototype.step = function() {
    this.life--;
    this.px = this.x;
    this.py = this.y;
    this.rad += opts.radVel;
    this.len -= this.lenVel;
    this.x = Math.cos(this.rad) * this.len;
    this.y = Math.sin(this.rad) * this.len;
    if (this.life <= 0)
      this.reset();
  };
  Line.prototype.draw = function() {
    const ratio = Math.abs(this.life / this.originalLife - 0.5);
    ctx.lineWidth = ratio * 5;
    ctx.strokeStyle = ctx.shadowColor = 'hsla(' +
        (tick + this.x / (w * (opts.ellipseBaseRadius + opts.ellipseAddedRadius)) * 100) +
        ', 80%, ' + (75 - ratio * 150) +
        '%, ' + this.alpha + ')';
    ctx.beginPath();
    ctx.moveTo(this.px, this.py);
    ctx.lineTo(this.x, this.y);
    ctx.stroke();
  };

  function Star() {
    this.reset();
  }
  Star.prototype.reset = function() {
    this.x = Math.random() * w;
    this.y = Math.random() * h;
    this.life = opts.starBaseLife + Math.random() * opts.starAddedLife;
  };
  Star.prototype.step = function() {
    this.life--;
    if (this.life <= 0)
      this.reset();
  };
  Star.prototype.draw = function() {
    ctx.fillStyle = ctx.shadowColor = 'hsla(' +
        (tick + this.x / w * 100) + ', 80%, 50%, 0.2)';
    ctx.shadowBlur = this.life;
    ctx.fillRect(this.x, this.y, 1, 1);
  };

  function resizeHandler() {
    w = canvas.width = window.innerWidth;
    h = canvas.height = window.innerHeight;
    opts.ellipseCX = w / 2;
    opts.ellipseCY = h / 2;
    init();
  }
  window.addEventListener('resize', resizeHandler);

  window.LuukLamers = function() {
    let i = 0,
        array = [300, 74, 0.04, 0.1, 0.1, 0.55, 10, 100, 10, -0.15, 0.18, 0.01, 3, 1, w / 2, h / 2, 0.02];
    for (let key in opts) {
      opts[key] = array[i];
      i++;
    }
    init();
  };

  init();

  onUnmounted(() => {
    window.removeEventListener('resize', resizeHandler);
    if (gui) {
      gui.destroy();
    }
  });
});

const startAnimationLoop = async () => {
  while (true) {
    await animateText();
    await new Promise(resolve => setTimeout(resolve, delayBetweenLoops));

  }
};

const animateText = async () => {
  isAnimating.value = true;
  await expandAndSplit();
  await new Promise(resolve => setTimeout(resolve, delayBetweenLoops));
  await shrinkSpacing();
  isAnimating.value = false;
};

const expandAndSplit = () => {
  return new Promise((resolve) => {
    const startText = "Wecord"; // 初始文本
    const finalText = "We Record"; // 最终文本
    const totalDuration = 0.8; // 动画总时长
    const totalSteps = 4; // 总步骤数

    // 假设 text 是一个响应式变量
    text.value = startText;

    // 根据步骤返回当前的文本
    const computeText = (step) => {
      if (step < 1) {
        return startText;
      } else if (step < 2) {
        return "We"+" "+"cord"
      } else if(step<3){
        return "We"+" "+"ecord";
      } else {
        return finalText;
      }
    };

    // GSAP timeline 用于同步控制 letterSpacing 动画
    const tl = gsap.timeline({
      onComplete: resolve
    });
    tl.to(letterSpacing, {
      value: "0px",
      duration: 0.5,
      ease: "power3.out"
    })
        .to(letterSpacing, {
          value: "0px",
          duration: 0.5,
          ease: "power2.inOut"
        }, "-=0.5");

    // 使用 GSAP tween 进行动画
    gsap.to({ progress: 0 }, {
      progress: 1,
      duration: totalDuration,
      ease: "none",
      onUpdate: function() {
        const progress = this.targets()[0].progress;
        const step = Math.floor(progress * totalSteps);
        text.value = computeText(step);
      },
      onComplete: resolve
    });
  });
};




const shrinkSpacing = () => {
  return new Promise((resolve) => {
    const startText = "We Record"; // 初始文本
    const totalDuration = 0.5; // 动画总时长
    const totalSteps = 4; // 3 次删除和 1 次替换，共 4 步

    // 假设 text 是一个响应式变量
    text.value = startText;

    // 根据步骤返回当前的文本
    const computeText = (step) => {
      let str = startText;
      // 第一步：删除原始索引 2 处的字符（第一次删除）
      if (step >= 1) {
        str = str.slice(0, 2) + str.slice(3);
      }
      // 第二步：再删除新文本中索引 2 的字符（第二次删除）
      if (step >= 2) {
        str = str.slice(0, 2) + str.slice(3);
      }
      // 第三步：再删除新文本中索引 2 的字符（第三次删除）
      if (step >= 3) {
        str = str.slice(0, 2) + str.slice(3);
      }
      return str;
    };

    // 使用 GSAP tween 进行动画
    gsap.to({ progress: 0 }, {
      progress: 1,
      duration: totalDuration,
      ease: "none",
      onUpdate: function() {
        const progress = this.targets()[0].progress;
        const step = Math.floor(progress * totalSteps);
        text.value = computeText(step);
      },
      onComplete: resolve
    });
  });
};










const resetText = () => {
  text.value = 'WeCord';
  letterSpacing.value = '0px';
};

// 联系表单相关
const contactForm = ref({
  name: '',
  email: '',
  message: ''
});
</script>

<template>
  <div class="app">
    <!-- Canvas 背景 -->
    <canvas id="c"></canvas>

    <!-- 固定导航栏 -->
    <header class="nav-header">
      <div class="container">
        <h2 class="logo">Wecord</h2>
        <nav>
          <ul>
            <li><a href="#hero">首页</a></li>
            <li><a href="#features">功能</a></li>
            <li><a href="#about">关于我们</a></li>
            <li><a href="#contact">联系</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <!-- 首页 -->
    <section id="hero" class="hero">
      <div class="content">
        <div id="inside" >
          <div style="color:#E16363" >Y</div>
          <div style="color:#FFB74D" >o</div>
          <div style="color:#9FA8DA" >u</div>
          <div style="width:30px"></div>
          <div style="color:#E16363" >C</div>
          <div style="color:#EC407A" >h</div>
          <div style="color:#FFCC80" >a</div>
          <div style="color:#FFFFFF" >t</div>

        </div>
        <div class="title" :style="{ letterSpacing: letterSpacing }">{{ text }}</div>
        <TextCarousel/>
        <button class="cta-button">
          <svg class="download-icon" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" width="20" height="20">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v2a2 2 0 002 2h12a2 2 0 002-2v-2M12 12v9m0 0l-3-3m3 3l3-3M12 3v9" />
          </svg>
          下载压缩包
        </button>


      </div>
    </section>

    <!-- 功能区 -->
    <section id="features" class="features">
      <div class="container">
        <OptimizedWaterfallJournal />

        <NotificationExtractor/>
      </div>
    </section>

    <!-- 关于我们 -->
    <section id="about" class="about">
      <div class="container">
        <h2>关于我们</h2>
        <p>
          WeCord 致力于为用户提供安全、高效的聊天记录存储与管理服务。我们的团队由经验丰富的开发人员和设计师组成，
          致力于通过创新技术，为用户提供安全、高效的聊天记录存储与管理服务，帮助您轻松记录生活点滴并规划日常任务。
        </p>
      </div>
    </section>

    <!-- 联系我们 -->
    <section id="contact" class="contact">
      <div class="container">
        <h2>联系我们</h2>
        <p>
          如有任何疑问，请发送邮件至
          <a href="WeCord@gmail.com" style="color: #4F46E5;">wecord@bjtu.com</a>
          与我们联系。
        </p>
      </div>
    </section>
  </div>
</template>

<style>
::-webkit-scrollbar {
  display: none;
}

html, body, #app {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, #1f1c2c, #928dab);
}
canvas {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 0;
}
.features h3 {
  color: #333; /* 深色标题 */
}

.features p {
  color: #555; /* 稍浅的段落文字 */
}

.contact p {
  text-align: center;
}

.features .container,
.about .container,
.contact,
.container{
  background: transparent; /* 将背景设置为透明 */
  color: #fff;
  width:100%;
  max-width:100%;
  position:relative;
  z-index:1;
}

/* 导航栏 */
.nav-header {
  position: fixed;
  top: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255,255,255,0.2);
  z-index: 10;
}
.nav-header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
}
.nav-header .logo {
  font-size: 1.8rem;
  color: #fff;
  font-weight: 700;
}
.nav-header nav ul {
  list-style: none;
  display: flex;
  gap: 1.5rem;
  margin: 0;
  padding: 0;
}
.nav-header nav ul li a {
  position: relative;
  background: transparent;
  color: #fff;
  text-decoration: none;
  font-size: 1rem;
  transition: color 0.3s ease;
}

/* 添加下划线伪元素 */
.nav-header nav ul li a::before,
.nav-header nav ul li a::after {
  content: '';
  position: absolute;
  width: 0%;
  height: 4px;
  bottom: -2px;
  background: #fff;
  transition: width 0.8s cubic-bezier(0.22, 0.61, 0.36, 1);
}

.nav-header nav ul li a::before {
  left: -2.5px;
}

.nav-header nav ul li a::after {
  right: 2.5px;
}

/* 鼠标悬停时动画 */
.nav-header nav ul li a:hover {
  background: transparent;
  color: #c8a2c8;
}

.nav-header nav ul li a:hover::before {
  width: 100%;
  transition: width 0.5s cubic-bezier(0.22, 0.61, 0.36, 1);
}

.nav-header nav ul li a:hover::after {
  width: 100%;
  background: transparent;
  transition: 0s;
}

/* 首页 */
.hero {
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  height: calc(100vh - 80px);
  margin-top: 80px;
  text-align: center;
  position: relative;
  z-index: 2;
}
.hero .content {
  width: 100%;
  max-width: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}
.title {
  font-size: 4rem;
  font-weight: 500;
  background-image: linear-gradient(45deg, #ffffff, #ddd);
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  text-shadow: 4px 4px 12px rgba(0, 0, 0, 0.5);
  transition: letter-spacing 0.5s ease;
  margin: 0;
}
.subtitle {
  font-size: 1.5rem;
  margin: 1rem 0 2rem;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.5);
}

#inside{
  display: flex;
  flex-direction: row;
  font-size: 4rem;
}
.cta-button {
  padding: 16px 48px;
  font-size: 1.25rem;
  font-weight: 500;
  color: #fff;
  background: linear-gradient(45deg, #4F46E5, #6366F1);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  box-shadow: 0px 8px 24px rgba(0, 0, 0, 0.3);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  margin-top:40px;
}
.cta-button:hover {
  transform: translateY(-5px);
  box-shadow: 0px 12px 32px rgba(0, 0, 0, 0.4);
}
.cta-button:active {
  transform: translateY(2px);
  box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.2);
}

/* 功能区 */
.features {
  padding: 4rem 0;
  color: #333;
  width:100%;
}
.features h2 {
  text-align: center;
  margin-bottom: 2rem;
}
.feature-items {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem;
}
.feature-item {
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0px 4px 16px rgba(0,0,0,0.1);
  text-align: center;
  flex: 1 1 300px;
}
.feature-item .icon {
  font-size: 3rem;
  margin-bottom: 1rem;
  color: #4F46E5;
}

/* 关于我们 */
.about {
  padding: 4rem 0;
  color: #333;
}
.about h2 {
  text-align: center;
  margin-bottom: 2rem;
}
.about p {
  max-width: 800px;
  margin: 0 auto;
  line-height: 1.6;
}

/* 联系我们 */
.contact {
  padding: 4rem 0;
  color: #333;
}
.contact h2 {
  text-align: center;
  margin-bottom: 2rem;
}
.contact form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 600px;
  margin: 0 auto;
}
.contact form input,
.contact form textarea {
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}
.contact form button {
  padding: 1rem;
  font-size: 1.2rem;
  font-weight: 500;
  color: #fff;
  background: #4F46E5;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: background 0.3s ease;
}
.contact form button:hover {
  background: #6366F1;
}

/* 通用容器 */
.container {
  width:100%;
  margin:0;
}

/* 响应式 */
@media (min-width: 1600px) {
  .title {
    font-size: 4rem;
  }
  .subtitle {
    font-size: 2rem;
  }
  .cta-button {
    font-size: 1.5rem;
    padding: 20px 60px;
  }
}

@media (min-width: 2560px) {
  .title {
    font-size: 8rem;
  }
  .subtitle {
    font-size: 2.5rem;
  }
  .cta-button {
    font-size: 1.75rem;
    padding: 24px 80px;
  }
}

</style>
