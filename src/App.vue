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
  document.documentElement.style.overflow = 'hidden';

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
    <!-- Canvas 与背景图片 -->
    <canvas id="c"></canvas>
    <a href="http://hd4desktop.com/669-colours-circle-rainbow-hd-wallpaper/"></a>
    <!-- 新的内容容器 -->
    <div class="content">
      <div class="header">
        <h1 class="title" :style="{ letterSpacing: letterSpacing }">{{ text }}</h1>
        <p class="subtitle">Your conversations, safely stored and easily accessible.</p>
      </div>
      <div class="bottom-info">
        <button class="cta-button">Get Started</button>
      </div>
    </div>
  </div>
</template>


<style>
html, body, #app {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  overflow: hidden;
  margin: 0;
  padding: 0;
}
canvas {
  position: absolute;
  top: 0;
  left: 0;
  background-color: black;
}
a {
  width: 200px;
  height: 200px;
  position: absolute;
  left: calc(100% - 200px);
  top: calc(100% - 130px);
}
img {
  width: 200px;
}
.app {
  max-width: none;
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.content {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  height: 100%;
  text-align: center;
  color: #ffffff;
  font-family: 'Poppins', sans-serif;
}

.title {
  font-size: 5rem;
  font-weight: 700;
  background-image: linear-gradient(45deg, #ffffff, #dddddd);
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  text-shadow: 3px 3px 8px rgba(255, 255, 255, 0.15);
  transition: letter-spacing 0.5s ease;
}

.subtitle {
  font-size: 1.5rem;
  font-weight: 400;
  color: #cccccc;
  text-shadow: 1px 1px 4px rgba(255, 255, 255, 0.1);
}

.cta-button {
  padding: 16px 48px;
  font-size: 1.25rem;
  font-weight: 500;
  color: #fff;
  background-color: #4F46E5;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  box-shadow: 0px 8px 24px rgba(79, 70, 229, 0.6);
  transition: all 0.3s ease;
}

.cta-button:hover {
  background-color: #6366F1;
  transform: translateY(-4px);
  box-shadow: 0px 12px 32px rgba(99, 102, 241, 0.6);
}

.cta-button:active {
  transform: translateY(2px);
  box-shadow: 0px 4px 16px rgba(79, 70, 229, 0.4);
}

@media (min-width: 1600px) {
  .title {
    font-size: 6rem;
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