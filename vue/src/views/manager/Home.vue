<template>
  <div>

    <div class="card" style="line-height: 30px; margin-bottom: 10px">
      <div style="margin-bottom: 20px; font-size: 24px">校园小卖部（软件项目管理第五组）</div>
      <div>技术栈：SpringBoot3 + Vue3 + MySQL，前后端分离</div>
      <div>目标旨在做一套完整的前后端分离的项目，<b>同时完成毕业设计</b></div>
    </div>

  </div>
</template>

<script setup>
import { reactive } from "vue";
import request from "@/utils/request";

const data = reactive({
  user: JSON.parse(localStorage.getItem('system-user') || '{}'),
})
</script>