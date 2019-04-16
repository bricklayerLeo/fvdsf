<template>
 <div>
  <swiper :lunbotuList="lunbotuList" :isfull="true"></swiper>

<ul class="mui-table-view mui-grid-view mui-grid-9">
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="/home/newslist">
		                    <img src="../../img/menu1.png" alt="">
		                    <div class="mui-media-body">新闻资讯</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="/home/photolist">
		                    <img src="../../img/menu2.png" alt="">
		                    <div class="mui-media-body">图片共享</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="/home/goodslist">
		                    <img src="../../img/menu3.png" alt="">
		                    <div class="mui-media-body">商品购买</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
		                    <img src="../../img/menu4.png" alt="">
		                    <div class="mui-media-body">留言反馈</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
		                   <img src="../../img/menu5.png" alt="">
		                    <div class="mui-media-body">视频更新</div></router-link></li>
		            <li class="mui-table-view-cell mui-media mui-col-xs-4 mui-col-sm-3"><router-link to="#">
		                    <img src="../../img/menu6.png" alt="">
		                    <div class="mui-media-body">联系我们</div></router-link></li>
		            
		        </ul> 

 </div>
</template>

<script>
import {Toast} from 'mint-ui'
import swiper from '../subcomponents/swiper.vue'
export default{
    data(){
        return{
            lunbotuList:[]
        }
    },
    created(){
        this.getlunbotu()
    },
    methods:{
        getlunbotu(){
            this.$http.get('api/getlunbo').then(result=>{
               // console.log(result.body)
               if(result.body.status === 0){
                   this.lunbotuList = result.body.message;
                   Toast('加在图片ok')
               }else{
                   Toast('加在图片失败')
               }
            })
        }
    },
    components:{
        swiper
    }
}

</script>

<style lang="scss" scoped>

.mui-grid-view.mui-grid-9{
    background-color: white;
    border:none;

    img{
        width:60px;
        height:60px;
    }

 
 
}

.mui-table-view.mui-grid-view .mui-table-view-cell{
font-size: 8px;
}

.mui-grid-view.mui-grid-9 .mui-table-view-cell {
    border-right: 0;
    border-bottom: 0;
 
}

</style>
