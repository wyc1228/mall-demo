<template>
  <router-view v-slot="{ Component }">
    <keep-alive>
      <component :is="Component" />
    </keep-alive>
  </router-view>
<!--  <keep-alive>-->
<!--    <router-view/>-->
<!--  </keep-alive>-->
  <main-tab-bar/>
</template>

<script>
import MainTabBar from "@/components/content/mainTabbar/MainTabBar";

export default {
  name: 'App',
  components: {
    MainTabBar
  }
}
</script>

<style>
@import "assets/css/base.css";
</style>
