<i18n>
en:
  render: Render
  render1: Fast Render
  rpe: RPE
  tasks: Tasks
  about: About

zh-CN:
  render: 渲染
  rpe: RPE
  tasks: 任务列表
  about: 关于

</i18n>

<script lang="ts">
import { ref, watch } from 'vue';
import { useRoute, useRouter } from 'vue-router';

import { useI18n } from 'vue-i18n';

import { VSonner } from 'vuetify-sonner';

const onLoaded = ref<() => void>();
const component = ref();

watch(component, (comp) => {
  if (comp && onLoaded.value) onLoaded.value();
});

export function useOnLoaded() {
  return onLoaded;
}

declare global {
  interface Window {
    goto: (name: string) => void;
  }
}

export default {
  data() {
    return {
      drawer: true,
    };
  },
  methods: {
    toggleNav() {
      this.drawer = !this.drawer;
    },
  },

};
</script>

<script setup lang="ts">
const { t } = useI18n();

const route = useRoute(),
  router = useRouter();

const icons = {
  render: 'mdi-auto-fix',
  rpe: 'mdi-bookshelf',
  tasks: 'mdi-server',
  about: 'mdi-information-outline',
};

window.goto = (name: string) => {
  router.push({ name });
};
</script>

<template>
  <v-app id="phi-recorder" class="dark-theme">
    <v-sonner position="top-center" />
    <v-app-bar :elevation="0" class="app-bar-shadow blur-background">
      <!--<v-app-bar-nav-icon @click="toggleNav" class="mx-1"></v-app-bar-nav-icon>-->
      <div class="gradient-text">
        <v-app-bar-title class="mx-5 text-glow">Phi Recorder</v-app-bar-title>
      </div>
    </v-app-bar>
    <v-navigation-drawer v-model="drawer" expand-on-hover rail permanent class="nav-drawer-border blur-background list-item">
      <v-list density="compact" nav>
        <v-list-item
          v-for="key in ['render', 'rpe', 'tasks', 'about']"
          :active="route.name === key"
          :key="key"
          :prepend-icon="icons[key as keyof typeof icons]"
          :title="t(key)"
          @click="router.push({ name: key })"
          class="list-item-hover"
          ></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="d-flex justify-center">
      <router-view v-slot="{ Component }">
        <Suspense timeout="0">
          <template #default>
            <component :is="Component" ref="component" />
          </template>
          <template #fallback>
            <div class="flex justify-center pa-8">
              <v-progress-circular class="waitIn" indeterminate size="large" />
            </div>
          </template>
        </Suspense>
      </router-view>
    </v-main>
  </v-app>
</template>

<style>

::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background-color: rgba(54, 50, 98, 0.1);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background-color: rgba(99, 102, 241, 0.5);
  border-radius: 4px;
  transition: background-color 0.3s;
}

::-webkit-scrollbar-thumb:hover {
  background-color: rgba(139, 92, 246, 0.7);
}

html {
  scroll-behavior: smooth;
}

/* make all text unselectable */
* {
  -webkit-user-select: none;
  /* Safari */
  -moz-user-select: none;
  /* Firefox */
  -ms-user-select: none;
  /* IE and Edge */
  user-select: none;
  /* Non-prefixed version, currently supported by Chrome, Opera, and Edge */
}

.v-input__details {
  /* 不让details占据布局空间 */
  display: none;
}

.v-form {
  scrollbar-width: thin;
  /* Firefox */
  scrollbar-color: rgba(99, 102, 241, 0.5) rgba(54, 50, 98, 0.1);
  /* Firefox */
}

.v-overlay-container {
  z-index: 1000;
  /* 设置遮罩层的背景颜色,使其与主题一致 */
  background-color: rgba(56, 56, 146, 0.8);
}

.waitIn {
  visibility: hidden;
  animation: Up 0.5s 0.5s forwards;
}

@keyframes Up {
  to {
    visibility: visible;
  }
}

.dark-theme {
  background: linear-gradient(45deg, #292364, #302b63, #24243e);
}

.app-bar-shadow {
  box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.2) !important;
}

.nav-drawer-border {
  border-right: 1px solid rgba(255, 255, 255, 0.1) !important;
}

.gradient-text {
  background: linear-gradient(30deg, #04c9ff, #ff13d4);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.list-item {
  transition: all 0.3s cubic-bezier(0.2, 0, 0.1, 1);
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2) !important;
}

.list-item-hover {
  transition: all 0.3s ease;
  margin: 8px 0px;
  border-radius: 12px;
}

.list-item-hover:hover {
  background: rgba(255, 255, 255, 0.05) !important;
  margin: 8px 4px;
  transform: translateX(4px);
  filter: drop-shadow(0 0 8px #ffffff8a);
}

.active-item {
  background: linear-gradient(45deg, rgba(33, 150, 243, 0.2), transparent) !important;
}

.active-item:hover {
  background: linear-gradient(45deg, rgba(33, 150, 243, 0.2), transparent) !important;
}

.glow-spinner {
  filter: drop-shadow(0 0 8px #2196F3);
}

.animated-background {
  position: relative;
  overflow: hidden;
}

.animated-background::before {
  content: '';
  position: absolute;
  width: 200%;
  height: 200%;
  background: linear-gradient(
    45deg,
    rgba(255, 255, 255, 0.01) 25%,
    transparent 25%,
    transparent 50%,
    rgba(255, 255, 255, 0.01) 50%,
    rgba(255, 255, 255, 0.01) 75%,
    transparent 75%,
    transparent
  );
  animation: animateFlow 2s linear infinite;
  opacity: 0.1;
}

@keyframes animateFlow {
  0% { transform: translate(-25%, -25%) rotate(0deg); }
  100% { transform: translate(-25%, -25%) rotate(360deg); }
}

.blur-background {
  backdrop-filter: blur(40px) saturate(180%);
  background: linear-gradient(45deg, rgba(122, 98, 168, 0.4), rgba(107, 66, 182, 0.4)) !important;
  transform: translateZ(0);
  position: relative;
  z-index: 1;
}
  
/* 新增的过渡动画样式 */
.fade-blur-enter-active,
.fade-blur-leave-active {
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  position: absolute;
  width: 100%;
}

.fade-blur-enter-from {
  opacity: 0;
  filter: blur(10px);
  transform: translateY(-20px);
}

.fade-blur-leave-to {
  opacity: 0;
  filter: blur(10px);
  transform: translateY(20px);
}

/* 新增的加载遮罩层样式 */
.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  backdrop-filter: blur(20px);
  background: rgba(16, 16, 36, 0.8);
  z-index: 999;
  display: flex;
  align-items: center;
  justify-content: center;
}

html {
  scrollbar-width: none;
  -ms-overflow-style: none;
}

html::-webkit-scrollbar {
  display: none;
}
</style>