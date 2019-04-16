<template>
    <div class="photo_container">
        <div class="mui-scroll-wrapper mui-slider-indicator mui-segmented-control mui-segmented-control-inverted">
           <div class="mui-scroll">
                  <a :class="['mui-control-item',item.id == 0 ? 'mui-active' :'']" 
                  v-for="item in  category" :key="item.id" @click="getPhotoListByCateId(item.id)" >
                   {{item.title}}
                     </a>
           </div> 
        </div>
              <ul class="haa">
                <router-link  v-for="item in list" :key="item.id" :to="'/home/photoinfo/'+item.id" tag="li">
                   <img v-lazy="item.img_url">
                   <div class="info">
                    <h1 class="title"> {{item.title}}</h1>
                          <div class="body">{{item.zhaiyao}}</div>
                
                   </div>
                </router-link>
            </ul>
    </div>
</template>

<script>
import mui from '../../lib/mui/js/mui.min.js'


export default {
    data(){
        return{
            category:[],
            list:[]
        }
    },
    created(){
        this.getAllCategory()
        this.getPhotoListByCateId(0)
    },
    mounted(){
        mui('.mui-scroll-wrapper').scroll({
	deceleration: 0.0005 //flick 减速系数，系数越大，滚动速度越慢，滚动距离越小，默认值0.0006
});
    },
    methods: {
        getAllCategory(){
            this.$http.get('api/getimgcategory').then(result=>{
               // console.log(result.body)
               if(result.body.status === 0){
                   result.body.message.unshift({title:"全部",id:0})
                   this.category = result.body.message;
                   
                        }else{
                          Toast('加在图片失败')
                       }
            })
        },
        getPhotoListByCateId(cateId){
            this.$http.get('api/getimages/' + cateId).then(result=>{
                 if(result.body.status === 0){
                   this.list= result.body.message;
                  
               }else{
                   Toast('加在图片失败')
               }
            })
        }
    }
}
</script>

<style lang="scss" scoped>
*{touch-action:none;}
//.photo_container{
//    
//}
.info{
    color: white;
    text-align: left;
    position:absolute;
    bottom: 0;
    background-color:rgba(0,0,0,0.4);
    max-height:  84px;
    .title{
      font-size: 14px;
    }
     .body{
       font-size: 13px;
     }
}
.haa{
    list-style: none;
    margin:0;
    padding: 10px; 
    img{
        width:100%;
        vertical-align: middle;
        }
    
    li{
        background-color: #ccc;
        text-align: center;
        margin-bottom: 5px;
        box-shadow:0 0 6px #999;
        position: relative;
          img[lazy=loading] {
          width: 40px;
          height: 300px;
          margin: auto;

          
           }
        }
     }


</style>
