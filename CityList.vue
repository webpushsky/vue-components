<template>
  <div class="city-list-wrap" ref="wrapper">
    <div>
      <div class="common-city width100  position-r">
        <div class="city-icon fontsize14 grayblackcolor padding-lr16 p-top10 p-bottom10"><img class="m-right6" src="../assets/img/hoteldetails/city-icon.png">常用国家</div>
        <div class="padding-lr16 blackcolor bg-white">
          <div class="hq-line-crosswise-one hq-height-line"></div>
          <div class="hq-line-crosswise-two hq-height-line"></div>
          <div class="hq-line-crosswise-three hq-height-line"></div>
          <div class="hq-line-crosswise-four hq-height-line"></div>
          <span class="inline-block aligncenter fontsize14 blackcolor" :class="{'vux-1px-r': (index + 1)%3 != 0, 'bluecolor': nationality === item.country_name}" v-for="(item, index) in hot" :key="index"  :id="item.iso_code" @click.native="handeClick">{{item.country_name}}</span>
        </div>
      </div>
      <div class="content">
        <div class="area" v-for="(item, key) in list"  :ref="key" :key="key">
          <div class="title border-topbottom padding-lr16 bg-e5e5e5 grayblackcolor fontsize14 vux-1px-b">{{key}}</div>
          <ul class="item-list bg-white">
              <li class="item border-bottom padding-lr16 blackcolor fontsize14 vux-1px-b" :class="{'bluecolor': nationality === listInner.country_name}" v-for="listInner in item" :key="listInner.iso_code"  @touchstart="handleTouchStart"  @click.native="handeClick"  @touchend= "handleTouchEnd" :id="listInner.iso_code">{{listInner.country_name}}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  props: {
    hot: Array,
    list: Object,
    letter: String
  },
  data () {
    return {
      touchstart: false,
      startY: 0,
      timer: null,
      nationality: ''
    }
  },
  computed: {
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.wrapper)
  },
  watch: {
    // 监听列表滚动事件 A-Z
    letter () {
      if (this.letter) {
        console.log(this.$refs)
        const element = this.$refs[this.letter][0]
        this.scroll.scrollToElement(element)
      }
    }
  },
  methods: {
    handeClick (e) {
      // 传给父组件
      console.log(123)
      this.$emit('chang', {'cityEn': e.target.id, 'cityCn': e.target.innerText})
    },
    handleTouchStart () {
      // 手指放上
      this.touchstart = true
    },
    handleTouchEnd () {
      // 手指离开
      this.touchstart = false
    }
  },
  created () {
    if (JSON.parse(sessionStorage.getItem('citizenship')) && JSON.parse(sessionStorage.getItem('citizenship')).cityCn) {
      this.nationality = JSON.parse(sessionStorage.getItem('citizenship')).cityCn
    }
  }
}
</script>

<style lang="scss">
@import '../assets/css/hq-base.scss';
.city-list-wrap {
  display: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  width: 100%;
  overflow:hidden;
  .content {
    .area {
      .title {
        height: 30 * $hq-base-1px;
        line-height: 30 * $hq-base-1px;
      }
      .item {
        text-overflow: ellipsis;
        white-space: nowrap;
        height: 40 * $hq-base-1px;
        line-height: 40 * $hq-base-1px;
      }
    }
  }
}
</style>
