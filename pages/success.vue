<template>
    <div>
        <div class="ui container">
            <div class="checkout-con">
               <div class="ui huge green message">
                   order completed successfully
               </div>
               <br>
               <br>
               <div>
                   <nuxt-link to="/">
                       <button class="fluid ui button">Go home</button>
                   </nuxt-link>
               </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    mounted(){
        this.refreshpage();
    },
    methods:{
        refreshpage(){
            
        }
    }
}
</script>

<style scoped>
.checkout-con{
    margin-top: 110px;
}
</style>