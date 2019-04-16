<template>
    <div class="goodslist-container">

      <transition 
      @before-enter = "beforeEnter"
      @enter = "Enter"
      @after-enter = "afterEnter">
       <div class="ball" v-show="ballFlag" ref="ball"></div>
      </transition>
     
      <!--商品轮播-->
        <div class="mui-card">
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<swiper :lunbotuList="lunbotu"  :isfull="false"></swiper>
					</div>
				</div>
        </div>
   <!--购买区域-->
      <div class="mui-card">
				<div class="mui-card-header">{{goodsinfo.title}}</div>
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
						<p class="price">
              <span >市场价:<del>￥{{goodsinfo.market_price}}</del></span>
              销售价:<span class="now_sell">￥{{goodsinfo.sell_price}}</span>
            </p>
            <p>购买数量：<numbox @getcount="getSelectedCount"
            :max="goodsinfo.stock_quantity"></numbox></p>
            <p>
              <mt-button type="primary" size="small">立即购买</mt-button>
              <mt-button type="danger" size="small" @click="goumai">加入购物车</mt-button>
              </p>
			  	</div>
			  </div>
      </div>
       <!--商品参数区域-->
       <div class="mui-card">
			<div class="mui-card-header">商品参数</div>
			<div class="mui-card-content">
				<div class="mui-card-content-inner">
				<p>商品货号：{{goodsinfo.goods_no}}</p>
        <p>库存情况：{{goodsinfo.stock_quantity}}件</p>
        <p>上架时间：{{goodsinfo.add_time | dateformat}}</p>
				</div>
			</div>
			<div class="mui-card-footer">
        <mt-button type="primary" size="large" plain @click="godesc(id)">图文介绍</mt-button>
        <mt-button type="danger" size="large" plain @click="gocomment(id)">商品点评</mt-button>
      </div>
		</div>

</div>
</template>

<script>
import swiper from '../subcomponents/swiper.vue'
import numbox from '../subcomponents/goodsinfo_number.vue'
export default {
    data(){
        return{
          id:this.$route.params.id,
          lunbotu:[],
          goodsinfo:{},
          ballFlag:false,
          selectedCount:1
        }
    },
    created(){
         this.getlunbotu()
         this.getGoodsinfo()
    },
    methods:{
        getlunbotu(){
            this.$http.get('api/getthumimages/' + this.id).then(result=>{
               if(result.body.status === 0){
                result.body.message.forEach(item => {
                    item.img = item.src
                });
                   this.lunbotu = result.body.message;
                   
                        }
            })
        },
        getGoodsinfo(){
            this.$http.get('api/goods/getinfo/' + this.id).then(result=>{
               // console.log(result.body)
               if(result.body.status === 0){
                   this.goodsinfo = result.body.message[0];
                   
               }
            })
        },
        godesc(id){
           this.$router.push({ name: 'goodsdesc', params: { id: id }})
        },
        gocomment(id){
           this.$router.push({ name: 'goodscomt', params: { id: id }})
        },
        goumai(){
            this.ballFlag=! this.ballFlag

            var goodsinfo = {id:this.id, count:this.selectedCount,
            price:this.goodsinfo.sell_price,selected:true}

            this.$store.commit('addtoCar',goodsinfo)
        },
        beforeEnter(el){
          el.style.transform = "translate(0,0)"
        },
        Enter(el,done){
          el.offsetWidth;

             const ballPosition = this.$refs.ball.getBoundingClientRect()
             const  badgePosition = document.getElementById('badge').getBoundingClientRect()

             const yDist = badgePosition.top - ballPosition.top
             const xDist = badgePosition.left - ballPosition.left

            
            el.style.transform = `translate(${xDist}px,${yDist}px)`
            el.style.transition = "all 1s cubic-bezier(.17,.67,.26,.85)"
            done()
        },
        afterEnter(el){
             this.ballFlag =! this.ballFlag
        },
        getSelectedCount(count){
             this.selectedCount = count;
             console.log(`从子组件拿到的纸是:${this.selectedCount}`)
        }
    },
    components:{
        swiper,
        numbox
    }
}
</script>


<style lang="scss" scoped>
.goodslist-container{
    background-color: #eee;
    overflow: hidden;
    .now_sell{
      font-size: 16px;
      font-weight: bold;
      color:red;
     
      }
       .mui-card-footer{
        display: block;
        button{
          margin: 15px 0;
        }
    }

.ball{
  width:15px;
  height:15px;
  border-radius:50%;
  background-color: red;
  position: absolute;
  z-index: 99;
  top: 390px;
  left:149px;
 
}

}

</style>
