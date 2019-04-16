<template>
    
       <div class="mui-numbox" data-numbox-minu='1'>
		    
					<button class="mui-btn mui-btn-numbox-minus" type="button">-</button>
					<input class="mui-input-numbox" type="number" :value="initcount"
                    @change="countChanged" ref="numbox" readonly/>
					<button class="mui-btn mui-btn-numbox-plus" type="button">+</button>
				</div>
    
</template>

<script>
import mui from "../../lib/mui/js/mui.min.js"
export default {
	mounted(){
		mui(".mui-numbox").numbox()
		
	},
    methods:{
		countChanged(){
            console.log(this.$refs.numbox.value)
             this.$store.commit("updateGoodsinfo",{id:this.goodsid,
             count:this.$refs.numbox.value
             })
        }
    },
    props:["initcount","goodsid"]
}
//:data-number-max='max'
</script>

<style lang="scss" scoped>

</style>
