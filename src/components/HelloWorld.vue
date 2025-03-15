<!--<script setup>-->
<!--import { ref, onMounted } from 'vue';-->
<!--import gsap from 'gsap';-->

<!--const text = ref('WeCord');-->
<!--const finalText = 'You Chat We Record';-->
<!--const isAnimating = ref(false);-->
<!--const letterSpacing = ref('0px');-->
<!--const delayBetweenLoops = 1500;-->

<!--onMounted(() => {-->
<!--  if (window) {-->
<!--    window.scrollTo({-->
<!--      left: document.body.scrollWidth,-->
<!--      behavior: 'instant'-->
<!--    });-->
<!--  }-->
<!--  startAnimationLoop();-->
<!--  document.documentElement.style.overflow = 'hidden';-->
<!--});-->

<!--const startAnimationLoop = async () => {-->
<!--  while (true) {-->
<!--    await animateText();-->
<!--    await new Promise(resolve => setTimeout(resolve, delayBetweenLoops));-->
<!--  }-->
<!--};-->

<!--const animateText = async () => {-->
<!--  isAnimating.value = true;-->
<!--  await expandAndSplit();-->
<!--  await shrinkSpacing();-->
<!--  resetText();-->
<!--  isAnimating.value = false;-->
<!--};-->

<!--const expandAndSplit = () => {-->
<!--  return new Promise((resolve) => {-->
<!--    let currentIndex = 0;-->
<!--    const targetSpacing = 16;-->
<!--    const startText = 'WeCord';-->

<!--    gsap.to(letterSpacing, {-->
<!--      value: `${targetSpacing}px`,-->
<!--      duration: 1.5,-->
<!--      ease: 'power3.out'-->
<!--    });-->

<!--    const interval = setInterval(() => {-->
<!--      if (currentIndex < startText.length) {-->
<!--        text.value = startText.substring(0, currentIndex + 1);-->
<!--      } else if (currentIndex < finalText.length) {-->
<!--        text.value = finalText.substring(0, currentIndex + 1);-->
<!--      }-->

<!--      currentIndex++;-->

<!--      if (currentIndex > finalText.length) {-->
<!--        clearInterval(interval);-->
<!--        resolve();-->
<!--      }-->
<!--    }, 80);-->
<!--  });-->
<!--};-->

<!--const shrinkSpacing = () => {-->
<!--  return new Promise((resolve) => {-->
<!--    gsap.to(letterSpacing, {-->
<!--      value: `4px`,-->
<!--      duration: 1.2,-->
<!--      ease: 'power2.inOut',-->
<!--      onComplete: resolve-->
<!--    });-->
<!--  });-->
<!--};-->

<!--const resetText = () => {-->
<!--  text.value = 'WeCord';-->
<!--  letterSpacing.value = '0px';-->
<!--};-->
<!--</script>-->

<!--<template>-->
<!--  <main class="container">-->
<!--    <div class="header">-->
<!--      <h1 class="title" :style="{ letterSpacing: letterSpacing }">{{ text }}</h1>-->
<!--      <p class="subtitle">Your conversations, safely stored and easily accessible.</p>-->
<!--    </div>-->
<!--    <div class="bottom-info">-->
<!--      <button class="cta-button">Get Started</button>-->
<!--    </div>-->
<!--  </main>-->
<!--</template>-->

<!--<style scoped>-->
<!--.container {-->
<!--  width: 100vw;-->
<!--  height: 100vh;-->
<!--  background: linear-gradient(-45deg, #1e1e1e, #282828, #1a1a1a);-->
<!--  background-size: 400% 400%;-->
<!--  display: flex;-->
<!--  flex-direction: column;-->
<!--  justify-content: center;-->
<!--  align-items: center;-->
<!--  color: #ffffff;-->
<!--  font-family: 'Poppins', sans-serif;-->
<!--  animation: gradientBackground 10s ease infinite;-->
<!--}-->

<!--html,body {-->
<!--  margin: 0;-->
<!--  padding: 0;-->
<!--}-->

<!--@keyframes gradientBackground {-->
<!--  0% { background-position: 0% 50%; }-->
<!--  50% { background-position: 100% 50%; }-->
<!--  100% { background-position: 0% 50%; }-->
<!--}-->

<!--.header {-->
<!--  text-align: center;-->
<!--  margin-bottom: 2rem;-->
<!--}-->

<!--.title {-->
<!--  font-size: 5rem;-->
<!--  font-weight: 700;-->
<!--  background-image: linear-gradient(45deg, #ffffff, #dddddd);-->
<!--  background-clip: text;-->
<!--  -webkit-background-clip: text;-->
<!--  color: transparent;-->
<!--  text-shadow: 3px 3px 8px rgba(255, 255, 255, 0.15);-->
<!--  transition: letter-spacing 0.5s ease;-->
<!--}-->

<!--.subtitle {-->
<!--  font-size: 1.5rem;-->
<!--  font-weight: 400;-->
<!--  color: #cccccc;-->
<!--  margin-top: 1rem;-->
<!--  text-shadow: 1px 1px 4px rgba(255, 255, 255, 0.1);-->
<!--}-->

<!--.bottom-info {-->
<!--  margin-top: 2rem;-->
<!--}-->

<!--.cta-button {-->
<!--  padding: 16px 48px;-->
<!--  font-size: 1.25rem;-->
<!--  font-weight: 500;-->
<!--  color: #fff;-->
<!--  background-color: #4F46E5;-->
<!--  border: none;-->
<!--  border-radius: 50px;-->
<!--  cursor: pointer;-->
<!--  box-shadow: 0px 8px 24px rgba(79, 70, 229, 0.6);-->
<!--  transition: all 0.3s ease;-->
<!--}-->

<!--.cta-button:hover {-->
<!--  background-color: #6366F1;-->
<!--  transform: translateY(-4px);-->
<!--  box-shadow: 0px 12px 32px rgba(99, 102, 241, 0.6);-->
<!--}-->

<!--.cta-button:active {-->
<!--  transform: translateY(2px);-->
<!--  box-shadow: 0px 4px 16px rgba(79, 70, 229, 0.4);-->
<!--}-->

<!--@media (min-width: 1600px) {-->
<!--  .title {-->
<!--    font-size: 6rem;-->
<!--  }-->
<!--  .subtitle {-->
<!--    font-size: 2rem;-->
<!--  }-->
<!--  .cta-button {-->
<!--    font-size: 1.5rem;-->
<!--    padding: 20px 60px;-->
<!--  }-->
<!--}-->

<!--@media (min-width: 2560px) {-->
<!--  .title {-->
<!--    font-size: 8rem;-->
<!--  }-->
<!--  .subtitle {-->
<!--    font-size: 2.5rem;-->
<!--  }-->
<!--  .cta-button {-->
<!--    font-size: 1.75rem;-->
<!--    padding: 24px 80px;-->
<!--  }-->
<!--}-->
<!--</style>-->
<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';

const text = ref('WeCord');
const finalText = 'You Chat We Record';
const isAnimating = ref(false);
const letterSpacing = ref('0px');
const delayBetweenLoops = 1500;

onMounted(() => {
  if (window) {
    window.scrollTo({
      left: document.body.scrollWidth,
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
  await shrinkSpacing();
  resetText();
  isAnimating.value = false;
};

const expandAndSplit = () => {
  return new Promise((resolve) => {
    let currentIndex = 0;
    const targetSpacing = 16;
    const startText = 'WeCord';

    gsap.to(letterSpacing, {
      value: `${targetSpacing}px`,
      duration: 1.5,
      ease: 'power3.out'
    });

    const interval = setInterval(() => {
      if (currentIndex < startText.length) {
        text.value = startText.substring(0, currentIndex + 1);
      } else if (currentIndex < finalText.length) {
        text.value = finalText.substring(0, currentIndex + 1);
      }
      currentIndex++;
      if (currentIndex > finalText.length) {
        clearInterval(interval);
        resolve();
      }
    }, 80);
  });
};

const shrinkSpacing = () => {
  return new Promise((resolve) => {
    gsap.to(letterSpacing, {
      value: `4px`,
      duration: 1.2,
      ease: 'power2.inOut',
      onComplete: resolve
    });
  });
};

const resetText = () => {
  text.value = 'WeCord';
  letterSpacing.value = '0px';
};
</script>

<template>
  <div class="app">
    <!-- Canvas 背景 -->
    <canvas id="c"></canvas>

    <!-- 固定导航栏 -->
    <header class="nav-header">
      <div class="container">
        <h2 class="logo">WeCord</h2>
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

    <section id="hero" class="hero">
      <div class="content">
        <h1 class="title" :style="{ letterSpacing: letterSpacing }">{{ text }}</h1>
        <p class="subtitle">Your conversations, safely stored and easily accessible.</p>
        <button class="cta-button">Get Started</button>
      </div>
    </section>

  </div>
</template>

<style>
html, body, #app {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
  background: linear-gradient(135deg, #1f1c2c, #928dab);
  overflow-x: hidden;
}
canvas {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 0;
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
  color: #fff;
  text-decoration: none;
  font-size: 1rem;
  transition: color 0.3s ease;
}
.nav-header nav ul li a:hover {
  color: #c8a2c8;
}

.hero {
  width: 100vw; /* 使用视口宽度确保占满全屏 */
  display: flex;
  align-items: center;
  justify-content: center;
  height: calc(100vh - 80px); /* 假设导航栏高度为80px */
  margin-top: 80px;
  text-align: center;
  position: relative;
  z-index: 2;
}

.hero .content {
  width: 100%; /* 内部内容全宽 */
  max-width: none; /* 取消可能的宽度限制 */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}



.title {
  font-size: 4rem;
  font-weight: 700;
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
}
.cta-button:hover {
  transform: translateY(-5px);
  box-shadow: 0px 12px 32px rgba(0, 0, 0, 0.4);
}
.cta-button:active {
  transform: translateY(2px);
  box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.2);
}

/* 通用容器 */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.feature-item .icon {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  color: #c8a2c8;
}

.about h2 {
  font-size: 2.8rem;
  margin-bottom: 1.5rem;
}
.about p {
  font-size: 1.1rem;
  line-height: 1.8;
  max-width: 800px;
  margin: 0 auto;
}

.contact h2 {
  font-size: 2.8rem;
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
  border: none;
  border-radius: 8px;
  font-size: 1rem;
}
.contact form button {
  padding: 1rem;
  font-size: 1.2rem;
  font-weight: 500;
  color: #fff;
  background: linear-gradient(45deg, #4F46E5, #6366F1);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: background 0.3s ease;
}
.contact form button:hover {
  background: linear-gradient(45deg, #6366F1, #4F46E5);
}

/* 页脚 */
.footer {
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  text-align: center;
  padding: 1.5rem 0;
}
.footer p {
  margin: 0;
  font-size: 0.9rem;
}

/* 响应式 */
@media (min-width: 1600px) {
  .title {
    font-size: 5rem;
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
