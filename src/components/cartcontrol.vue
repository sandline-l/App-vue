<template>
    <div class="cartcontrol">
        <span class="decrease icon-remove_circle_outline" v-show="food.count>0" @click='decrease'></span>
        <span class="count" v-show="food.count>0">{{food.count}}</span>
        <span class="increase icon-add_circle" @click='add'></span>
    </div>
</template>
<script>
import Vue from 'vue'
export default {
    name:'cartcontrol',
    methods:{
        add () {
            if(!this.food.count){
                Vue.set(this.food, "count")
                this.food.count=1
            }else{
                this.food.count++
            }
        },
        decrease () {
            if(this.food.count>0){
                this.food.count--
            }
        }
    },
    props:{
        food:{
            type:Object
        }
  }
}
</script>
<style lang="stylus" ref='stylesheet/stylus'>
@import '../assets/stylus/index.styl'
    .cartcontrol
        position absolute
        right 18px
        bottom 18px
        .decrease,.increase
            font-size 24px
            color rgb(0,160,220)
            line-height 24px
        .count
            font-size 16px
            color rgb(147,153,159)
            line-height 24px
            text-align center
            width 24px
            vertical-align top
            display inline-block
</style>
