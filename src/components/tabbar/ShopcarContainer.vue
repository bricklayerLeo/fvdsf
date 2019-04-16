<template>
 <div class="shopcar-container">
   
         <div class="goodslis">
                <div class="mui-card" v-for="(item,i) in goodslist" :key="item.id">
				<div class="mui-card-content">
					<div class="mui-card-content-inner">
                      
						 <mt-switch v-model="$store.getters.getGoodsSelected[item.id]"
                         @change="selectedChanged(item.id,$store.getters.getGoodsSelected[item.id])"> </mt-switch>
                           <img :src="item.thumb_path"  alt="">
                           <div class="info">
                               <h1>{{item.title}}</h1>
                             
                                   <p>
                                       <span class="price">￥{{item.sell_price}}</span>
                                       <numbox :initcount="$store.getters.getGoodsCount[item.id]" :goodsid="item.id"></numbox>
                                       <a href="" @click.prevent="remove(item.id,i)">删除</a>
                                   </p>
                                     </div>
					</div>
				</div>
                </div>
                </div>

                <div class="mui-card">
				<div class="mui-card-content">
					<div class="mui-card-content-inner  jiesuan">
						<div class="wenzi">
                            <p>总计（不含运费）</p>
                            <p>已勾选商品<span class="jianshu">{{this.$store.getters.getGoodsCountAndAmount.count}}</span>件，总价<span class="jianshu">￥{{this.$store.getters.getGoodsCountAndAmount.amount}}</span>元</p>
                        </div>
					 <mt-button type="danger" size="normal">结算</mt-button>
                    </div>
				</div>
                </div>
 </div>
</template>

<script>
import numbox from '../subcomponents/shopcar_numbox.vue'
export default{
    data(){
        return{
            goodslist:[]
        }
    },
    created(){
        this.getshopcarlist()
              
    },
    methods:{
        getshopcarlist(){
            var idArr=[]
            this.$store.state.car.forEach(item => {
                idArr.push(item.id)
                if(idArr.length <=0){
                    return
                }
            });
            this.$http.get('api/goods/getshopcarlist/'+idArr.join(",")).then(result=>{
               // console.log(result.body)
               if(result.body.status === 0){
                   this.goodslist = result.body.message;
               } 
            })
            
        },
        remove(id,index){
            this.goodslist.splice(index,1)
            this.$store.commit('removeFormCar',id)
        },
        selectedChanged(id,val){
            this.$store.commit("uodateGoodsSelected",{id,selected:val})
        }
    },
 components:{
     numbox
 }
}
</script>

<style lang="scss" scoped>
.shopcar-container{
 background-color: #eee;
 overflow: hidden;

 .goodslis{
     .mui-card-content-inner{
         display: flex;
         align-items: center;
     }
      
    
    
     img{
         width:60px;
         height:60px;
     }
     h1{
         font-size: 13px;
     }
     .info{
         display: felx;
         flex-direction: column;
         justify-content: space-between;
         .price{
         color: red;
         font-weight: bold;
         
     }
     
     
 }
 }
 .mui-card-content-inner{
     display: flex;
     justify-content: space-between;
     align-items: center;//居中
     font-size: 16px;
     .jianshu{
         color: red;
         font-weight: bold;
     }
 }
 
}
</style>
