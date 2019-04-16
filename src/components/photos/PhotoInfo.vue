<template>
    <div class="photoinfo_container">
        <h3>{{photoinfo.title}}</h3>
        <p class="subtitle">
            <span>发表时间：{{photoinfo.add_time | dateformat}}</span>
            <span>点击次数：{{photoinfo.click}}次</span>
        </p>

        <hr>

        <vue-preview
      :list="list"
      :thumbImageStyle="{width: '80px', height: '80px', margin: '10px'}"
      :previewBoxStyle="{border: '1px solid #eee'}"
      :tapToClose="true"
      @close="closeHandler"
      @destroy="destroyHandler"
    />

       <div class="content" v-html="photoinfo.content"></div>
<cmt-box :id="this.id"></cmt-box>
    </div>
</template>

<script>
import comment from '../subcomponents/comment.vue'
export default {
    data(){
        return{
            id:this.$route.params.id,
            photoinfo:{},
            list:[]
        }
    },
    created(){
        this.getPhotoInfo()
        this.getThumbs()
    },
    methods:{
        getPhotoInfo(){
            this.$http.get("api/getimageInfo/" + this.id)

            .then(result=>{
                 if(result.body.status === 0){
                   this.photoinfo= result.body.message[0];
                  
               
               }
            })
        },
        getThumbs(){
            this.$http.get("api/getthumimages/" + this.id)

            .then(result=>{
                 if(result.body.status === 0){
                    result.body.message.forEach(item => {
                        item.w=600
                        item.h=400
                    });
                  
                    this.list =  result.body.message
               }
            })

        },

    closeHandler() {
      console.log('closeHandler')
    },
    // 完全关闭之后，调用这个函数清理资源
    destroyHandler() {
      console.log('destroyHandler')
    }
    },
    components:{
        'cmt-box':comment
    }
}
</script>

<style lang="scss" scoped>
.photoinfo_container{
    padding:3px;
    h3{
        color: #26a2ff;
        font-size: 15px;
        text-align: center;
        margin:15px 0;
    }
    .subtitle{
        display: flex;
        justify-content: space-between;
        font-size: 13px;
    }
    .content{
       font-size: 13px;
       line-height: 30px; 
    }
}
</style>
