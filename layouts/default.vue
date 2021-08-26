<template>
<div>
  <div class="default-con">
        <!-- top buttons -->
        <div>
        <div class="ui secondary  menu">
            <span @click="goBack" class="top-back"> 
                <i class="bordered teal angle left icon"></i>
            </span>
           
        <div class="right menu">
            <nuxt-link to="/cart" class="ui item">
                <span class="material-icons">add_shopping_cart</span> 
                <span>( {{cartTotalLength}} )</span>
                <span>Ksh {{ cartTotalPrice.toFixed(0) }}</span>
            </nuxt-link>
        </div>
     </div> 
    </div>
    <!-- end of top buttons -->
    <!-- SEARCH -------- START ---->
    <div>
        <div class="ui search">
        <div class="ui icon input">
                <input class="prompt search-input" type="text" placeholder="search...">
                <i class="search icon"></i>
        </div>
        <div class="results"></div>
    </div>

    </div>
    <!-- SEARCH -------- END ---->
  </div>

    <div>
        <nuxt/>
    </div>
</div>
</template>

<script>
export default {
    data(){
        return{
            cart: {
                items: []
            },
        }
    },

    mounted(){
        this.cart = this.$store.state.cart;
    },

    methods:{
        goBack(){
            // this.$router.go(-1)
        }
    },

    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.sell_price * curVal.quantity
            }, 0)
        },
}
    
}
</script>
<style scoped>
.default-con{
    background:#ffa64d;
    position: fixed;
    width: 100%;
    top: 0%;
    left: 0%;
    right: 0%;
    z-index: 1;
    height: 100px;
}
.ui.input{
    padding-top: 5px !important;
    padding-bottom: 10px !important;
    margin-left: 10px !important;
    margin-right: 10px !important;
    
}
.search-input{
    width: 90vw !important;
}
.top-back{
    margin-left: 20px;
    color: white;
    margin-top: 10px;
}
</style>