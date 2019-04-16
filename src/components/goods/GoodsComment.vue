<template>
    <div>
        <combox :id="this.$route.params.id"></combox>
    </div>
</template>

<script>
import combox from '../subcomponents/comment.vue'
export default {
    data(){
        return{}
    },
    methods:{

    },
    components:{
        combox
    }
}
</script>

<style lang="scss" scoped>

</style>
