<template>
	<div class="cartcontrol">
		<transition name="move">
			<div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="decreaseCart">
			   <!--<span class="inner "></span>-->
			</div>
		</transition>
		<div class="cart-count" v-show="food.count>0">{{ food.count }}</div>
		<div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
	</div>
</template>

<script>
	import Vue from 'vue'
	export default {
		props: {
			food: {
				type:Object
			}
		},
		methods:{
			addCart(event) {
//				if(!event._constructed){
//					return;
//				}
				if(!this.food.count){
					Vue.set(this.food,'count',1)
					this.food.count=1
				}else{
					this.food.count++
				}
				//向父组件派发事件，并传递 当前dom元素
				this.$emit('add',event.target)
			},
			decreaseCart() {
				if(this.food.count){
					this.food.count--
				}
			}
		},
		created() {
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
   .move-enter-active
      transition:  .7s 
      opacity:1
      transform: translate3d(0, 0, 0)
      .cart-decrease
       transform: rotate(0deg)
   .move-leave-active
      transition: .5s 
      opacity:0
   .move-enter,&.move-leave-to
      opacity:0
      transform: translate3d(24px, 0, 0)
      .cart-decrease
       transform: rotate(180deg)
   .cartcontrol
     font-size:0
     .cart-decrease
       display:inline-block
       padding:6px
       line-height: 24px
       font-size:24px
       color:rgb(0,160,220)
     .cart-count 
       display:inline-block
       vertical-align:top
       width:12px
       padding-top:6px
       line-height:24px
       text-align:center
       font-size:10px
       color:rgb(147,153,159)
     .cart-add
       display:inline-block
       padding:6px
       font-size:24px
       color:rgb(0,160,220)
      
</style>