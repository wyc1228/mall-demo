<template>
  <div id="home">
    <nav-bar class="home-nav">
      <template v-slot:center>
        <div>分类</div>
      </template>
    </nav-bar>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";
import BetterScroll from "better-scroll";

  export default {
    name: "Category",
    components: {
      NavBar,
      BetterScroll
    },
    data() {
      return {

      }
    },
    // 组件创建完之后才会调用(在模板渲染成html后调用)
    mounted() {
      let bs = new BetterScroll('.wrapper', {
        movable: true,
        zoom: true
      })
    },
    // 组件创建完之后才会调用(在模板渲染成html前调用)
    created() {
    }

  }
</script>

<style scoped>

</style>
