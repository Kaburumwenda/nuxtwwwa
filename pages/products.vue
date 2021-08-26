<template>
    <div class="ui container">
        <div>
          <div class="index-top-con">
           <div class="index-top-spacer">

             <!-- **************************** BODY ************** START ******** -->
             <div class="">
               <div  v-if="$store.state.isLoading">
               <div class="ui segment">
                    <div class="ui active loader"></div>
                    <p>Loading ...</p>
                </div>
             </div>
                <div v-else>
                <div class="animate__animated animate__fadeInUpBig">
                   <div v-if="products.length >0">
                   <div v-for="rs in products" :key="rs.id">
                    <div>
                      <div class="index-menu-items">
                        <div>
                            <img :src="rs.get_image" class="index-img">
                        </div>
                        <div>
                            <span>{{ rs.desc1}}</span> <br>
                            <span><strong>Ksh {{ rs.sell_price }}</strong></span>
                        </div>
                        <div>
                            <button class="ui circular cart plus icon primary button" @click.prevent="addToCart(rs)">
                                <i class="cart plus icon"></i>
                            </button>
                        </div>
                    </div>
                    </div>
                 </div>
                 </div>
                 <div v-else>
                   <center>
                        <div>
                            <img src="../assets/cat/camera-a.png" class="no-pro-img">
                         </div>
                         <h3><span class="no-pro-dis"><em>No items to display, Please explore other brands</em></span></h3>
                         <br>
                         <nuxt-link to="/"><b>Go to homepage</b></nuxt-link>
                   </center>
                   </div>
                </div>
                </div>
             </div>
              <!-- **************************** BODY ************** END ******** -->
           </div>
        </div>
        </div>
    </div>
</template>

<script>
export default {
  data(){
    return{
      products:{},
      quantity: 1,
      cart: {
          items: []
      },
    }
  },
 
  created(){
    this.getProduct();
  },
  mounted(){
    this.getProduct();
    this.cart = this.$store.state.cart;
  },

  methods:{
    async getProduct(){
      const categorySlug = this.$route.params.category_slug
      const subcategorySlug = this.$route.params.subcategory_slug
      const brandSlug = this.$route.params.brand_slug
      this.$store.commit('setIsLoading', true)
      await this.$axios.$get(`/api/v1/categorylist/${categorySlug}/${subcategorySlug}/${brandSlug}/`)
      .then(res =>{
          this.products = res;
          // document.title = 'products | Lubiri'
      })
     this.$store.commit('setIsLoading', false)
    },

    addToCart(rs) {
        const item = {
            product: rs,
            quantity: this.quantity
        }
        if (isNaN(this.quantity) || this.quantity < 1) {
        this.quantity = 1
        }
        this.$store.commit('addToCart', item) 
    },
    
  }
}
</script>

<style scoped>
.index-top-spacer{
    margin-top: 110px;
    /* margin-left: 10px;
    margin-right: 10px; */
    margin-bottom: 50px;
}
.index-top-con{
    background: rgba(242, 242, 242, 0.5);
}
.index-top-btn{
   box-shadow: 0 0 5px grey;
  -webkit-box-shadow: 0 0 5px grey;
  -moz-box-shadow: 0 0 5px grey;
}
.index-img{
    max-width: 120px;
    max-height: 100px;
}
.index-menu-items{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 10px;
    border-radius: 5px;
    padding:10px 10px 10px 10px;
    background: white;
    box-shadow: 0 0 5px grey;
    -webkit-box-shadow: 0 0 5px grey;
    -moz-box-shadow: 0 0 5px grey;
}
/***************************** NO PRODUCT CON */
.no-pro-img{
  max-width: 150px;
}
.no-pro-dis{
  font-family: 'Crimson Text', serif;
}
</style>