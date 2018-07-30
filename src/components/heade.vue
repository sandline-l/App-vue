<template>
  <div id="header">
    <div class="contant-wrapper">
      <div class="avatar" >
        <img :src= "seller.avatar" class="set-img">
      </div>
      <div class="contant">
        <div class="title">
          <span class="brand bg-image"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="coin bg-image" :class="classMap[seller.supports[0].type]"></span>
          <span>{{seller.supports[0].description}}</span>
        </div>
        <div class="num-wrapper" v-if="seller.supports">  
          <span class="num">{{seller.supports.length}}个</span>
          <span class="icon-keyboard_arrow_right more"></span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-icon"></span>
      <span class="bulletin">{{seller.bulletin}}</span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <div class="backgrand">
      <img :src= "seller.avatar" alt="">
    </div>
  </div>
</template>

<script>
export default {
  name: 'heade',
  props: {
    seller: {
      type: Object
    }
  },
  created () {
    this.classMap = ['decrease','discount','special','invoice_1','guarantee']
    // console.log(this.seller.score)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl'
// @import '../../static/style.css';
#header
  position relative
  overflow hidden
  .contant-wrapper
    position relative
    padding 24px 24px 18px
    background rgba(7,17,27,0.5)
    .avatar
      float left
      img 
        border-radius 2px
        set-img(64px)
    .contant
      margin-left 64px
      padding-left 16px
      color rgb(255,255,255)
      .title
        .brand
          display inline-block
          bg-image('brand')
          width 30px
          height 18px
          background-size 30px 18px
          vertical-align top
        .name
          font-size 16px
          line-height 16px
          font-weight bold  
      .description
        margin 8px 0 10px 
        font-size 12px
        line-height 12px
      .supports
        margin-top 10px
        font-size 10px
        span  
          display inline-block
        .coin
          width 16px
          height 16px
          vertical-align top
          display inline-block
          background-size 16px 16px
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.special
            bg-image('special_1')
          &.invoice_1
            bg-image('invoice_1')
          &.guarantee
            bg-image('guarantee_1')
      .num-wrapper
        color rgb(255,255,255)
        font-size 10px
        background-color rgba(0,0,0,0.2)
        height 12px
        line-height 12px
        position absolute
        right 12px
        bottom 14px
        padding 7px 8px
        border-radius 12px
        .num
        .more
          width 2px
          vertical-align middle
  .bulletin-wrapper
    position relative
    height 28px
    line-height 28px
    font-size 10px
    background-color rgba(7,17,27,0.2)
    padding 0 12px 0 12px
    white-space nowrap
    text-overflow ellipsis
    overflow hidden
    color white
    .bulletin-icon
      display inline-block
      vertical-align middle
      width 22px
      height 12px
      bg-image('bulletin')
      background-size 22px 12px
    .bulletin
      margin-left 4px
      margin-right 4px
      vertical-align middle
    .icon-keyboard_arrow_right
      position absolute
      right 12px
      bottom 8px
  .backgrand
    position absolute
    top 0
    left 0
    z-index -1
    width 100%
    filter blur(10px)
    img 
      background-size cover
      width 100%
</style>
