<template>
    <div id="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <div v-for="(item,index) in goods" :key="index" class="menu-item" :class="{current: currentIndex === index}" @click="selectIndex(index,$event)">
          <div class="text-wrapper border-1px">
            <span class="icon" v-show="item.type > 0" :class="classMap[item.type]"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </div>
      </div>
      <div class="foods-wrapper" ref="foodWrapper">
        <ul>
          <li v-for="(items,index) in goods" :key="index" class="food-list-hook">
            <div class="items-title">{{items.name}}</div>
            <ul>
              <li v-for="(item2,index) in items.foods" :key="index" class="items-content-wrapper">
                <div class="items-icon">
                  <img :src="item2.icon" alt="">
                </div>
                <div class="items-content border-1px">
                  <div class="items-name">{{item2.name}}</div>
                  <div v-show="item2.description" class="items-description">{{item2.description}}</div>
                  <div class="ratings">
                    <span>月售{{item2.sellCount}}份</span>
                    <span style="margin-left:12px">好评率{{item2.rating}}%</span>
                  </div>
                  <div class="prices">
                    <span class="price">￥{{item2.price}}</span>
                    <span v-if="item2.oldPrice" class="oldPrice" style="margin-left:8px">￥{{item2.oldPrice}}</span>
                  </div>
                </div>
                <cartcontrol :food = item2></cartcontrol>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart :selectFoods=selectFoods :minPrice = 'seller.minPrice' :deliveryPrice = 'seller.deliveryPrice'></shopcart>  
    </div>
</template>
<script>
import axios from 'axios'
import Vue from 'vue'
import BScroll from 'better-scroll'
import shopcart from './shopcart'
import cartcontrol from './cartcontrol'
export default {
  name : "goods",
  data () {
    return {
      goods : [],
      heightList:[],
      scrollY:0,
      currentIndex:0
    }
  },
  props:{
    seller:{
      type:Object
    }
  },
  components:{
    shopcart,
    cartcontrol
  },
  created () {
    this.classMap = ['decrease','discount','special','invoice_1','guarantee'];
    axios.get('/good/goods').then(res => {
      if(res.data.code === 0){
        this.goods = res.data.data;
        Vue.nextTick(() => {
          this._initscroll() //dom绑定一个scroll
        })
      }
    })
  },
  computed:{
    //循环goods数据里的food数据,将被挑选的food单独保存到数组中,方便之后在购物车使用这些数据
    //被挑选的food,在控制组件里,都加上了count属性
    selectFoods () {
      let foodArr=[];
      this.goods.forEach(good => {
        good.foods.forEach(food => {
          if(food.count){
            foodArr.push(food)
          }
        })
      })
      return foodArr
    }
  },
  methods: {
    //选择菜单index,并且根据这个index滚动到相应位置,并将该index传给当前index(currentIndex)
    selectIndex ($index,$event) {
      if(!$event._constructed){
        return;
      }
      let foodList=this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
      this.foodScroll.scrollToElement(foodList[$index],300);
      this.currentIndex=$index;
    },
    //better-scroll,给dom绑定scroll
    _initscroll(){
      this.menuScroll = new BScroll(this.$refs.menuWrapper,{
        click: true
      })
      this.foodScroll = new BScroll(this.$refs.foodWrapper,{
        probeType:3,
        click: true
      })
      this.foodScroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    }
  }
}
</script>
<style lang="stylus" ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl'
  #goods
    display flex
    position absolute
    top 174px
    bottom 48px
    left 0
    right 0
    width 100%
    overflow hidden
    .icon
      display inline-block
      vertical-align top
      width 14px
      height 14px
      background-size 14px 14px
      &.decrease
        bg-image('decrease_3')
      &.discount
        bg-image('discount_3')
      &.special
        bg-image('special_3')
      &.invoice_1
        bg-image('invoice_3')
      &.guarantee
        bg-image('guarantee_3')
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background-color #f3f5f7
      overflow auto 
      .menu-item
        padding 0 12px
        font-size 0px
        line-height 16px
        &.current
          position relative
          margin-top -1px
          background-color #fff
          &:after
            border-top 1px solid #ffffff
          .text
            font-size 12px
            line-height 14px
            font-weight 500
        .text
          font-size 12px
          line-height 14px
          font-weight 200
      .text-wrapper
        display table-cell
        vertical-align middle
        height 54px
        width 56px
        border-1px(rgba(7,17,27,0.2))
    .foods-wrapper
      flex 1
      position relative
      background-color #fff
      .items-title
        font-size 12px
        height 26px
        background-color #f3f5f7
        color rgb(147,153,159)
        line-height 26px
        border-left 4px solid #d9dde1 
        padding-left 14px
      .items-content-wrapper
        display flex
        margin 18px
        padding-bottom 18px
        position relative
        background #ffffff
        border-1px rgba(7,17,27,0.1)
        &:last-child
          border-1px(rgb(255,255,255))
          margin-bottom 0
        .items-icon
          flex 0 0 57px
          img 
            width 57px
            height 57px
        .items-content
          flex 1
          padding-left 10px
          .items-name 
            font-size 14px
            color rgb(7,17,27)
            line-height 14px
            margin-top 2px
          .items-description,.ratings
            margin-top 8px
            font-size 10px
            color rgb(147,153,159)
            line-height 16px
          .rating
            span 
              display inline-block
          .prices
            font-size 20px
            color rgb(240,20,20)
            font-weight 700
            line-height 24px
          .oldPrice
            font-size 10px
            vertical-align top
            color rgb(147,153,159)
            font-weight 700
            line-height 20px
            text-decoration line-through



</style>
