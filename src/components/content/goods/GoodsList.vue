<template>
<div class="goods-list">
  <goods-list-item v-for="item in goods" :goods-item="item" class="item"/>
</div>
</template>

<script>
import GoodsListItem from "./GoodsListItem";

export default {
  components: {
    GoodsListItem
  },
  name: "GoodsList",
  props: {
    goods: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style scoped>
.goods-list {
  display: flex;
  flex-wrap: wrap;
  padding: 5px;
  justify-content: space-around;
}

.goods-list .item {
  width: 48%;
}
</style>
