<template>
	<div class="ratings" ref="ratingWrapper">
		<div class="ratings-content">
			<div class="overview">
				<div class="overview-left">
					<h1 class="score">{{seller.score}}</h1>
					<div class="title">综合评分</div>
					<div class="rank">高于周边商家</div>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="title">服务态度</span>
						<star class="star" :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="title">商品评分</span>
						<star class="star"  :size="36" :score="seller.foodScore"></star>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="title">送达时间</span>
						<span class="delivery">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
			<split></split>
			<rating-select :select-type="selectType" 
				  :ratings="ratings" 
				:only-content="onlyContent"
				 :desc="desc"></rating-select>
		    <div class="rating-wrapper">
		    	<ul>
		    		<li v-for="rating in ratings">
		    			<div class="avatar">
		    				<img width="28" height="28" :src="rating.avatar" alt="" />
		    			</div>
		    			<div class="content">
		    				<h1 class="name">{{rating.username}}</h1>
		    				<div class="star-wrapper">
		    					<star :size="24" :score="rating.score"></star>
		    					<span class="delivery" v-show="rating.deliveryTime"></span>
		    				</div>
		    				<p class="text">{{rating.text}}</p>
		    				<div class="recommend" v-show="rating.recommend.length">
		    					<span class="icon-thumb_up"></span>
		    					<span  v-for="item in rating.recommend"></span>
		    				</div>
		    				<div class="time">{{rating.rateTime | formatDate}}</div>
		    			</div>
		    		</li>
		    	</ul>
		    </div>
		</div>

	</div>
</template>

<script>
	import BScroll from 'better-scroll'
	import star from '@/components/star/star'
	import ratingSelect from '@/components/ratingselect/ratingselect'
	import split from '@/components/split/split'
	import {formatDate} from '@/common/js/date'
	
//	const POSITIVE = 0
//	const NEGATIVE = 1
	const ALL = 2
	const ERR_OK = 0
	export default {
		data(){
			return {
				selectType: ALL,
				onlyContent: true,
				ratings:[]
			}
		},
		props:{
			seller:{
				type:Object
			}
		},
		components:{
			star,
			ratingSelect,
			split
		},
		created(){
			this.$http.get('/api/ratings').then((response)=> {
				response = response.body;
				if(response.errorno === ERR_OK){
					this.ratings = response.data
					this.$nextTick(()=>{
						if(!this.scorll){
							this.scroll = new BScroll(this.$refs.ratingWrapper,{
								click:true
							})
						}else{
							this.scroll.refresh()
						}
					})
				}
			})
		},
		filters:{
			formatDate(time){
				let date = new Date(time)
				return formatDate(date, 'yyyy-MM-dd hh:mm')
			}
		}
		
	}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratings
    position:absolute
    top:174px
    bottom:0
    left:0
    width:100%
    overflow:hidden
    .overview
      display: flex
      padding: 18px 0
      .overview-left
        flex:0 0 137px
        padding:6px 0
        width:137px
        border-right: 1px solid rgba(7, 17, 27,0.1)
        text-align:center
        @media only screen and (max-width: 320px){
        	flex: 0 0 110px;
        	width:110px
        }
        .score
          margin-bottom: 6px
          line-height: 28px
          font-size: 24px
          color: rgb(255, 153, 0)
        .title
          margin-bottom: 8px
          line-height:12px
          font-size: 12px
          color: rgb(7, 17, 27)
        .rank
          padding-bottom:6px
          margin-bottom: 8px
          line-height: 10px
          font-size: 10px
          color: rgb(147, 153,159)
      .overview-right
        flex: 1
        padding: 6px 0 6px 24px
        @media only screen and (max-width: 320px){
        	padding-left:6px
        }
        .score-wrapper
          margin-bottom: 8px
          font-size: 0
          .title
            display:inline-block
            vertical-align: top
            font-size: 12px
            line-height: 18px
            color: rgb(7, 17, 27)
          .star
            display:inline-block
            vertical-align: top
            margin: 0 12px
          .score
            display:inline-block
            vertical-align: top
            line-height: 18px
            font-size: 12px
            color: rgb(255,153,0)
         .delivery-wrapper
           font-size:0 
           .title
            font-size: 12px
            line-height: 18px
            color: rgb(7, 17, 27)
          .delivery
            font-size: 12px
            margin-left:12px
            color: rgb(147, 153, 159)
    .rating-wrapper
      padding: 0 18px
      display:flex
</style>