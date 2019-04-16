<template>
    <div class="desc-container">
        <h3>{{goodsdesc.title}}</h3>
        <hr>
<div class="content" v-html="goodsdesc.content"></div>
    </div>
</template>

<script>
export default {
    data(){
        return{
           goodsdesc:{},
           id:this.$route.params.id
        }
    },
    created(){
        this.getGoodsdesc()
    },
    methods:{
        getGoodsdesc(){
            this.$http.get('api/goods/getdesc/' + this.id).then(result=>{
               if(result.body.status === 0){
                   this.goodsdesc = result.body.message[0];
                   
               }
            })
        },
    }
}
</script>

<style lang="scss" >
.desc-container{
    h3{
        font-size: 16px;
        color: #226aff;
        text-align: center;
        //line-height: 30px;
        margin: 15px;
    }
    .content{
        img{
            width:100%;
        }
    }
}
</style>
