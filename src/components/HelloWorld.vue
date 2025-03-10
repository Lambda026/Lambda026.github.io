<script setup>
import { ref, onMounted } from 'vue';
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
  <main class="container">
    <div class="header">
      <h1 class="title" :style="{ letterSpacing: letterSpacing }">{{ text }}</h1>
      <p class="subtitle">Your conversations, safely stored and easily accessible.</p>
    </div>
    <div class="bottom-info">
      <button class="cta-button">Get Started</button>
    </div>
  </main>
</template>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(-45deg, #1e1e1e, #282828, #1a1a1a);
  background-size: 400% 400%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #ffffff;
  font-family: 'Poppins', sans-serif;
  animation: gradientBackground 10s ease infinite;
}

html,body {
  margin: 0;
  padding: 0;
}

@keyframes gradientBackground {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.header {
  text-align: center;
  margin-bottom: 2rem;
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
  margin-top: 1rem;
  text-shadow: 1px 1px 4px rgba(255, 255, 255, 0.1);
}

.bottom-info {
  margin-top: 2rem;
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
