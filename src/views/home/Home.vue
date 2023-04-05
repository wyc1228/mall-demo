<template>
  <div id="home">
    <nav-bar class="home-nav">
      <template v-slot:center>
        <div>购物街</div>
      </template>
    </nav-bar>
    <tab-control class="tab-control"  :titles="titles" @tabClick="tabClick" ref="tabControl1" v-show="isTabFixed"/>
    <scroll class="content" ref="scroll" :probe-type="3" :pull-up-load="true" @scroll="contentScroll" @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
      <recommend-view :recommends="recommends"/>
      <feature-view/>
      <tab-control :titles="titles" @tabClick="tabClick" ref="tabControl2"/>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click="backClick" v-show="isShowBackTop"/>
  </div>
</template>

<script>

import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl";
import GoodsList from "components/content/goods/GoodsList";
import Scroll from "components/common/scroll/Scroll";
import BackTop from "components/content/backTop/BackTop";

import {getHomeMultiData,getHomeGoods} from "network/home";
import {debounce} from "common/utils";

export default {
  name: "Home",
  components: {
    GoodsList,
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    Scroll,
    BackTop
  },
  data() {
    return {
      banners: [],
      recommends: [],
      titles: ["流行","新款","精选"],
      goods: {
        'pop':{page:0,list:[]},
        'new':{page:0,list:[]},
        'sell':{page:0,list:[]},
      },
      currentType: 'pop',
      isShowBackTop: false,
      tabOffsetTop: 0,
      isTabFixed: false,
      saveY:0
    }
  },
  /**
   * 计算属性
   */
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    }
  },
  // 活跃
  activated() {
    this.$refs.scroll.backTo(0,this.saveY,0)
    this.$refs.scroll.refresh()
  },
  deactivated() {
    this.saveY = this.$refs.scroll.getScrollY()
  },
  created() {
    // 请求多个数据
    this.getHomeMultiData();
    // 请求商品数据
    this.getHomeGoods('pop');
    this.getHomeGoods('new');
    this.getHomeGoods('sell');
  },
  mounted() {
    // 监听Item中图片加载完成
    // const refresh = debounce(this.$refs.scroll.refresh,500)
    // this.$bus.$on('itemImageLoad',()=>{
    //   // this.$refs.scroll.refresh()
    //   // console.log('itemImageLoad');
    //   // 防抖函数
    //   // refresh()
    // })
  },
  methods: {
    /**
     * 事件监听
     * */
    tabClick(index) {
      this.currentType = Object.keys(this.goods)[index];
      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
    },
    contentScroll(position) {
      // 判断backTop是否显示
      this.isShowBackTop = position.y < -1000
      // 判断tabControl是否吸顶 吸顶效果(position:fixed)
      this.isTabFixed = position.y < -this.tabOffsetTop
    },
    loadMore() {
      this.getHomeGoods(this.currentType)
    },
    swiperImageLoad() {
      // 所有组件都有一个属性 $el: 用于获取组件中到元素
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    },
    /**
     * 网络监听
     * */
    getHomeMultiData() {
      getHomeMultiData().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type,page).then(res => {
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
        // 每次加载完数据主动结束掉上拉加载方法，以便下次可以触发(Back-scroll插件规则)
        this.$refs.scroll.stopPullUp()
      })
    },
    backClick() {
      this.$refs.scroll.backTo(0,0,500)
    },

  }
}
</script>

<style scoped>
#home{
  /*padding-top: 44px;*/
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: var(--color-tint);
  color: #f6f6f6;

  /*position: fixed;*/
  /*left: 0;*/
  /*right: 0;*/
  /*top: 0;*/
  /*z-index: 9;*/
}
.tab-control{
  /*position: sticky;*/
  position: relative;
  /*top: 44px;*/
  z-index: 9;
}

.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}
</style>
