<template>
  <div class="titlebar" @mousedown="handleMouseDown">
    <div class="left no-drag">
      <history-control />
      <search-bar class="mg-left"
        v-if="route.name === 'FilmIndex' || route.name === 'IptvIndex' || route.name === 'AnalyzeIndex'" />
      <player-show class="mg-left" />
    </div>
    <div class="right no-drag">
      <div class="system-functions">
        <sponsor class="system-function" />
        <just-look class="system-function" />
        <language class="system-function" />
        <system-skin class="system-function" />
        <system-config class="system-function" />
      </div>
      <system-control v-if="platform !== 'darwin'" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router';

import HistoryControl from './HistoryControl.vue';
import SearchBar from './SearchBar.vue';
import PlayerShow from './PlayShow.vue';
import SystemConfig from './SystemConfig.vue';
import SystemControl from './SystemControl.vue';
import SystemSkin from './SystemSkin.vue';
import Sponsor from './Sponsor.vue';
import Language from './Language.vue';
import JustLook from './JustLook.vue'

const { platform } = window.electron.process;
const route = useRoute();

const handleMouseDown = (event) => {
  if (event.detail === 2) {
    window.electron.ipcRenderer.send('win:invoke', 'max');
  }
}
</script>

<style lang="less" scoped>
.titlebar {
  -webkit-app-region: drag;
  display: flex;
  justify-content: space-between;
  height: 32px;
  margin: var(--td-comp-margin-m) var(--td-comp-margin-xs) var(--td-comp-margin-m) 0;

  .no-drag {
    -webkit-app-region: no-drag;
  }

  .left {
    height: 100%;
    display: flex;

    .mg-left {
      margin-left: 20px;
    }
  }

  .center {
    margin-left: 20px;
  }

  .right {
    display: flex;

    .system-functions {
      display: flex;
      align-items: center;
      justify-content: space-around;

      .system-function {
        margin-left: var(--td-comp-margin-xs);
        width: 30px;
        height: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
      }
    }

    .system-controls {
      display: flex;
    }
  }
}
</style>
