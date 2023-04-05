<template>
<!-- ref 绑定组件 -->
<div class="wrapper" ref="wrapper">
  <div class="content">
    <slot></slot>
  </div>
</div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: "Scroll",
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      scroll: null
    }
  },
  mounted() {
    // 1、创建BScroll对象
    this.scroll = new BScroll(this.$refs.wrapper, {
      observeDOM:true,
      click:true,
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad
    })
    // 2、监听滚动位置
    this.scroll.on('scroll',(position) => {
      this.$emit('scroll',position)
    })
    // 3、监听上拉事件(监听是否滚动到底部)
    this.scroll.on('pullingUp',() => {
      this.$emit('pullingUp')
    })
    this.scroll.refresh()
  },
  methods: {
    // 回到顶部
    backTo(x,y,time=300) {
      // scroll 自带的scrollTo方法跳转到某个位置
      this.scroll && this.scroll.scrollTo(x,y,time)
    },
    // 主动结束上拉加载更多
    stopPullUp() {
      this.scroll && this.scroll.finishPullUp()
    },
    refresh() {
      this.scroll && this.scroll.refresh()
    },
    getScrollY() {
      return this.scroll ? this.scroll.y : 0
    }
  }
}
</script>

<style scoped>

</style>
