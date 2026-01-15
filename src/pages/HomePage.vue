<template>
    <main class="px-4 py-5 my-5 text-center" style="min-height: 50vh">
        <h1 class="display-5 fw-bold mt-4">Stay Organized, be productive</h1>
        <div class="scene">
            <div
            class="ball"
            :style="{
                transform: `translate(${x}px, ${y}px)`
            }"
            ></div>
        </div>
        <div class="col-lg-6 mx-auto">
            <p class="lead mb-4">
                Organize your ideas, and be productive everyday.
            </p>
            <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
                <router-link
                    :to="{ name: 'register' }"
                    class="btn btn-primary btn-lg px-4 gap-3"
                    >Sign up</router-link
                >
                <router-link
                    :to="{ name: 'login' }"
                    class="btn btn-outline-secondary btn-lg px-4"
                    >Sign in</router-link
                >
            </div>
        </div>
    </main>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const x = ref(0);
const y = ref(0);

let t = 0;
let rafId = null;

const amplitude = 80; // высота волны
const speed = 5;      // скорость по X

let direction = 1;   // 1 → вправо, -1 → влево

function animate() {
  const width = window.innerWidth - 30; // ширина экрана минус диаметр шара

  x.value += speed * direction;
  t += 0.05;

  y.value = Math.sin(t) * amplitude;

  if (x.value >= width) direction = -1;
  if (x.value <= 0) direction = 1;

  rafId = requestAnimationFrame(animate);
}

onMounted(() => {
  animate();
});

onUnmounted(() => {
  cancelAnimationFrame(rafId);
});
</script>

<style scoped>
.scene {
  position: relative;
  width: 100vw;
  height: 300px;
  overflow: hidden;
}

.ball {
  position: absolute;
  left: 0;
  top: 100px; /* базовая линия */
  width: 30px;
  height: 30px;
  background: #0b5ed7;
  border-radius: 50%;
}
</style>