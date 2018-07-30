<template>
    <div class="shopcart">
        <div class="content">
            <div class="chart-icon-wrapper" @click="goList">
                <div class="chart-icon icon-shopping_cart" :class="{bule:totalCount!=0}" ></div>
                <div class="total-count" v-show="totalCount!=0">{{totalCount}}</div>
            </div>
            <div class="deliver-fee" @click="goList">
                <div class="price" :class="{white:totalPrice!=0}">￥{{totalPrice}}</div>
                <div class="deliveryPrice">另需配送费￥{{deliveryPrice}}元</div>
            </div>
            <div class="deliver-base" :class="{pay:totalPrice>20}">{{deliverStr}}</div>
        </div>
        <div class="cart-list-wrapper" v-show="showList" ref='cartScroll'>
            <div class="cart-gray" @click="goList"></div>
            <div class="cart-list">
                <div class="cart-list-header">
                    <span class="cart-title">购物车</span>
                    <span class="clear" @click="clearAll">清空</span>
                 </div>
                <div class="food-item" v-for="(payfood,index) in selectFoods" :key="index" >
                    <span class="food-title">{{payfood.name}}</span>
                    <span class="food-list">￥{{payfood.price}}</span>
                    <cartcontrol class="cart-control" :food = payfood></cartcontrol>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Vue from 'vue'
import BScroll from 'better-scroll'
import cartcontrol from './cartcontrol'
export default {
    name:'shaopcart',
    data () {
        return {
            lock:true
        }
    },
    props:{
        minPrice:{
            type:Number
        },
        deliveryPrice:{
            type:Number
        },
        selectFoods:{
            type:Array
        }
  },
  created () {
    Vue.nextTick(()=>{
      this._initScroll()
    })
    
  },
  components:{
      cartcontrol
  },
  computed:{
      totalCount () {
            let totalCount=0;
            this.selectFoods.forEach(item => {
                totalCount+=item.count
            })
            return totalCount
      },
      totalPrice () {
            let totalPriceNum = 0;
            this.selectFoods.forEach(item => {
                totalPriceNum += item.count*item.price;
            })
            return totalPriceNum
      },
      deliverStr () {
          if(this.totalPrice != 0 && this.totalPrice < 20){
              return `还差${20-this.totalPrice}起送`
          }else if(this.totalPrice === 0){
              return  `￥${this.minPrice}起送`
          }else{
              return `去结算`
          }
      },
      //当食物数量不为空,隐藏购物车,当有食物时,将状态初始化为真,并且隐藏购物车
       showList () {
          if(!this.totalCount){
              this.lock=true
              return false
          }else{
              return !this.lock
          }
      }
  },
  methods:{
      //如果被选食物有值,那么就让lock(状态)取反,没有值就不作操作(返回空)
      goList () {
          if(!this.selectFoods.length){
              return 
          }
        this.lock=!this.lock
      },
      //清空购物车
      clearAll () {
          this.selectFoods.forEach(item => {
              item.count=0
          })
      },
      //使列表能够滚动
       _initScroll(){
      this.scroll = new BScroll(this.$refs.cartScroll,{
        click: true
      })
      this.scroll.on('scroll',(pos) => {})
    }
     
  }

}
</script>
<style lang="stylus" ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl'
    .shopcart
        position fixed
        height 48px
        width 100%
        bottom 0px
        left 0px
        z-index 300
        .content
            width 100%
            background-color #141d27
            display flex
            .chart-icon-wrapper
                flex 0 0 80px
                position relative
                .chart-icon
                    width 44px
                    height 44px
                    border-radius 44px
                    font-size 24px
                    line-height 44px
                    margin-left 18px
                    margin-top -10px
                    margin-bottom 8px
                    color rgba(255,255,255,0.4)
                    background-color #2b333b
                    text-align center  
                    &.bule
                        background-color rgb(0,160,220)  
                        color rgb(255,255,255)
                .total-count
                    display inline-block
                    color rgb(255,255,255)
                    background-color rgb(240,20,20)
                    line-height 16px
                    width 24px
                    height 16px
                    font-weight 700
                    font-size 8px
                    border-radius 16px
                    box-box-shadow 0px 4px 8px 0px rgba(0,0,0,0.4)
                    text-align center
                    position absolute
                    right 0px
                    top -6px
            .deliver-fee
                flex 1
                height 24px
                padding 12px 0 
                text-align center
                .price
                    display inline-block
                    line-height 24px
                    font-size 16px
                    font-weight 700
                    color rgba(255,255,255,0.2)
                    border-1px-right rgba(255,255,255,0.4)
                    padding-right 12px
                    &.white
                        color rgb(255,255,255)
                .deliveryPrice
                    display inline-block
                    padding-left 12px
                    color rgba(255,255,255,0.4)
                    font-size 12px
                    line-height 20px
                    font-weight 700
            .deliver-base
                flex 0 0 105px
                padding 0 8px
                font-size 12px
                color rgba(255,255,255,0.4)
                font-weight 700
                line-height 48px
                text-align center
                background-color: #2b333b;
                &.pay
                    background-color rgb(0,160,220)
                    color rgb(255,255,255)
        .cart-list-wrapper
            position fixed
            top 0px
            bottom 48px
            left 0px
            width 100%
            display flex
            flex-direction column
            z-index -1
            .cart-gray
                flex 1
                background-color rgba(7,17,27,0.6)
            .cart-list
                width 100%
                position fixed
                bottom 48px
                left 0
                background-color #fff
                overflow auto 
                max-height 217px
                .cart-list-header
                    height 40px
                    background-color #f3f5f7
                    padding 0 18px
                    border-bottom 1px solid rgba(7,17,27,0.1)
                    .cart-title
                        font-size 14px
                        font-weight 200
                        color rgb(7,17,27)
                        line-height 40px
                    .clear
                        font-size 12px
                        color rgb(0,160,220)
                        line-height 40px
                        position absolute
                        right 18px
                .food-item
                    height 24px
                    margin 0 18px
                    padding 12px 0
                    border-bottom 1px solid rgba(7,17,27,0.1)
                    position relative
                    .food-title
                        display inline-block
                        font-size 14px
                        height 24px
                        line-height 24px
                        color rgb(7,17,27)
                    .food-list
                        position absolute
                        right 120px
                        bottom 12px
                        height 24px
                        line-height 24px
                        font-size 20px
                        color rgb(240,20,20)
                        font-weight 700
                    .cart-control
                        position absolute
                        right 24px
                        bottom 12px





</style>
