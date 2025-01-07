<script setup>
    import { useRouter } from 'vue-router'
    const router = useRouter()
    function navTo(path) {
        router.push(path)
    }
</script>
<template>
    <div class="menu-demo">
        <a-menu mode="horizontal" theme="light" :default-selected-keys="['1']">
            <a-menu-item key="1" @click="navTo('/')">首页</a-menu-item>
            <a-menu-item key="2" @click="navTo('/cpu')">CPU监控</a-menu-item>
        </a-menu>
    </div>
</template>
<style scoped>
.menu-demo {
  box-sizing: border-box;
  width: 100%;
  background-color: var(--color-neutral-2);
}
</style>