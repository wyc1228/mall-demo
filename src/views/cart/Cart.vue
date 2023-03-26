<template>
  <div id="home">
    <nav-bar class="home-nav">
      <template v-slot:center>
        <div>购物车</div>
      </template>
    </nav-bar>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";

  export default {
    name: "Cart",
    components: {
      NavBar
    }
  }
</script>

<style scoped>

</style>
