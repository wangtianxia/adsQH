<template>
    <section class="alertContent">
        <img class="bg" src="static/alert.jpg" alt="">
        <img class="longTime" src="static/alertBtn2.png" alt="">
        <img class="closeImg" src="static/closeAlert.gif" @click="closeAlert" alt="">
    </section>
</template>
<script>
export default {
    data(){
        return {

        }
    },
    methods:{
        closeAlert(){
            this.$emit('closeAlert')
        }
    }
}
</script>
<style scoped>
    .alertContent{
        position: fixed;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        left: 0;
        top: 0;
        z-index: 100;
        background-color: rgba(0,0,0,0.5);
        flex-wrap: wrap;
        align-content: center;
    }
    .bg{
       width: 4.77rem;
       height: 8.16rem;
    }
    .longTime{
        width: 4.77rem;
        margin-top: 0.2rem;
    }
    .closeImg{
        position: absolute;
        width: 0.4rem;
        height: 0.4rem;
        right: 1.5rem;
        top: 1rem;
    }
</style>

