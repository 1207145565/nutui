<template>
  <div class="demo">
    <h2>基本用法</h2>
    <nut-cell title="我是标题" desc="描述文字"></nut-cell>
    <nut-cell title="我是标题" sub-title="副标题描述" desc="描述文字"></nut-cell>
    <nut-cell title="点击测试" @click="testClick"></nut-cell>
    <h2>链接</h2>
    <nut-cell title="链接" is-link></nut-cell>
    <nut-cell title="URL 跳转" desc="https://jd.com" is-link url="https://jd.com"></nut-cell>
    <nut-cell title="路由跳转 ’/‘ " to="/"></nut-cell>
    <h2>展示图标</h2>
    <nut-cell title="姓名" icon="my" desc="张三"></nut-cell>
    <h2>只展示 desc ，可通过 desc-text-align 调整内容位置</h2>
    <nut-cell desc-text-align="left" desc="张三"></nut-cell>
  </div>
</template>

<script lang="ts">
import { createComponent } from '@/utils/create';
const { createDemo } = createComponent('cell');
export default createDemo({
  props: {},
  setup() {
    const testClick = (event: Event) => {
      alert(event);
    };
    return { testClick };
  }
});
</script>

<style lang="scss" scoped></style>
