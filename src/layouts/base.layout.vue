<script lang="ts" setup>
import { NIcon, useThemeVars } from 'naive-ui';

import { Home2, Menu2 } from '@vicons/tabler';
import { RouterLink } from 'vue-router';

import { storeToRefs } from 'pinia';
import HeroGradient from '../assets/hero-gradient.svg?component';
import MenuLayout from '../components/MenuLayout.vue';
import NavbarButtons from '../components/NavbarButtons.vue';
import CollapsibleToolMenu from '@/components/CollapsibleToolMenu.vue';
import { useStyleStore } from '@/stores/style.store';
import { useToolStore } from '@/tools/tools.store';
import type { ToolCategory } from '@/tools/tools.types';

const themeVars = useThemeVars();
const styleStore = useStyleStore();
const { t } = useI18n();

const toolStore = useToolStore();
const { favoriteTools, toolsByCategory } = storeToRefs(toolStore);

const tools = computed<ToolCategory[]>(() => [
  ...(favoriteTools.value.length > 0
    ? [{ name: t('tools.categories.favorite-tools'), components: favoriteTools.value }]
    : []),
  ...toolsByCategory.value,
]);
</script>

<template>
  <MenuLayout :class="{ isSmallScreen: styleStore.isSmallScreen }" class="menu-layout">
    <template #sider>
      <RouterLink class="hero-wrapper" to="/">
        <HeroGradient class="gradient" />
        <div class="text-wrapper">
          <div class="title">IT - TOOLS</div>
          <div class="divider" />
          <div class="subtitle">
            {{ $t('home.subtitle') }}
          </div>
        </div>
      </RouterLink>

      <div class="sider-content">
        <div v-if="styleStore.isSmallScreen" flex flex-col items-center>
          <div flex justify-center>
            <NavbarButtons />
          </div>
        </div>

        <CollapsibleToolMenu :tools-by-category="tools" />

        <div class="footer">
          <div>
            © {{ new Date().getFullYear() }}
            <c-link href="https://blog.zhaozhiwei.cc" rel="noopener" target="_blank"> blog.zhaozhiwei.cc </c-link>
          </div>
        </div>
      </div>
    </template>

    <template #content>
      <div flex gap-2 items-center justify-center>
        <c-button
          :aria-label="$t('home.toggleMenu')"
          circle
          variant="text"
          @click="styleStore.isMenuCollapsed = !styleStore.isMenuCollapsed"
        >
          <NIcon :component="Menu2" size="25" />
        </c-button>

        <c-tooltip :tooltip="$t('home.home')" position="bottom">
          <c-button :aria-label="$t('home.home')" circle to="/" variant="text">
            <NIcon :component="Home2" size="25" />
          </c-button>
        </c-tooltip>

        <command-palette />
        <div>
          <NavbarButtons v-if="!styleStore.isSmallScreen" />
        </div>
      </div>
      <slot />
    </template>
  </MenuLayout>
</template>

<style lang="less" scoped>
.support-button {
  background: rgb(37, 99, 108);
  background: linear-gradient(48deg, rgba(37, 99, 108, 1) 0%, rgba(59, 149, 111, 1) 60%, rgba(20, 160, 88, 1) 100%);
  color: #fff !important;
  transition: padding ease 0.2s !important;

  &:hover {
    color: #fff;
    padding-left: 30px;
    padding-right: 30px;
  }
}

.footer {
  text-align: center;
  color: #838587;
  margin-top: 20px;
  padding: 20px 0;
}

.sider-content {
  padding-top: 160px;
  padding-bottom: 200px;
}

.hero-wrapper {
  position: absolute;
  display: block;
  left: 0;
  width: 100%;
  z-index: 10;
  overflow: hidden;

  .gradient {
    margin-top: -65px;
  }

  .text-wrapper {
    position: absolute;
    left: 0;
    width: 100%;
    text-align: center;
    top: 16px;
    color: #fff;

    .title {
      font-size: 25px;
      font-weight: 600;
    }

    .divider {
      width: 50px;
      height: 2px;
      border-radius: 4px;
      background-color: v-bind('themeVars.primaryColor');
      margin: 0 auto 5px;
    }

    .subtitle {
      font-size: 16px;
    }
  }
}
</style>
