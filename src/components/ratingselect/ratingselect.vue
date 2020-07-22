<template>
	<div class="ratingselect">
		<div class="rating-type border-1px">
			<span class="block positive" @click="select(2,$event)" :class="{'active': myselectType ===2}">{{ desc.all }} <span class="count">{{ratings.length}}</span></span>
			<span class="block positive" @click="select(0,$event)" :class="{'active': myselectType ===0}">{{ desc.positive }} <span class="count">{{positives.length}}</span></span>
			<span class="block negative" @click="select(1,$event)" :class="{'active': myselectType ===1}"> {{ desc.negative }} <span class="count">{{negatives.length}}</span></span>
		</div>
		<div @click="toggleContent" class="switch" :class="{'on':myonlyContent}">
			<span class="icon-check_circle"></span>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script>
	const POSITIVE = 0
	const NEGATIVE = 1
	const ALL = 2;
	export default {
		props:{
			ratings: {
				type:Array,
				default(){
					return [];
				}
			},
			selectType: {
				type:Number,
				default:ALL
			},
			onlyContent: {
				type:Boolean,
				default: false
			},
			desc: {
				type: Object,
				default() {
					return {
						all: '全部',
						positive: '满意',
						negative: '不满意'
					}
				}
			}
		},
		data() {
			return {
				myselectType:this.selectType,
				myonlyContent:this.onlyContent
			}
		},
		computed: {
			positives(){
				return this.ratings.filter( (ele) => {
					return ele.rateType === POSITIVE
				})
			},
			negatives(){
				return this.ratings.filter( (ele) => {
					return ele.rateType === NEGATIVE
				})
			}
		},
		methods:{
			select(type,event){
//				if(!event._constructed){
//					return;
//				}
				this.myselectType = type
				this.$emit('select',type)
			},
			toggleContent(){
				this.myonlyContent = !this.myonlyContent
				this.$emit('togglecontent',this.myonlyContent)
			}
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"
	.ratingselect
	  .rating-type
	    padding: 18px 0
	    margin: 0 18px
	    border-1px(rgba(7,17,27,0.1))
	    font-size:0
	    .block
	      display:inline-block
	      padding:8px 12px
	      margin-right: 8px
	      line-height: 16px
	      border-radius: 1px
	      font-size:12px
	      color:rgb(77,85,93)
	      &.active
	        color:#fff
	      .count
	        font-size:10px
	        margin-left: 2px
	        transform:scale(0.8)
	      &.positive
	        background: rgba(0,160,220,0.2)
	        &.active
	          background: rgb(0,160,220)
	      &.negative
	        background: rgba(77,85,93,0.2)
	        &.active
	          background: rgb(77,85,93)
	 .switch
	   padding: 12px 18px
	   line-height: 24px
	   border-bottom: 1px solid rgba(7,17,27,0.1)
	   color: rba(147,153,159)
	   font-size:0
	   &.on
	     .icon-check_circle
	       color:#00c850
	   .icon-check_circle
	     vertical-align:top
	     display:inline-block
	     font-size: 24px
	     margin-right: 4px 
	   .text
	     font-size: 12px
	     vertical-align:top
	     display:inline-block
	     
</style>