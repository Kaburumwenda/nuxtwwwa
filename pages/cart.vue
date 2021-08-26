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
                    <div v-if="cart.length < 0">
                   <center>
                        <div>
                            <img src="../assets/cat/camera-a.png" class="no-pro-img">
                         </div>
                         <h3><span class="no-pro-dis"><em>No items to display, Please explore other brands</em></span></h3>
                         <br>
                         <nuxt-link to="/"><b>Go to homepage</b></nuxt-link>
                   </center>
                   </div>

                   <div v-else>
                   <div v-for="item in cart.items" :key="item.product.id">
                    <div class="index-menu-items-con">
                      <div class="index-menu-items">
                        <div>
                            <img :src="item.product.get_image" class="index-img">
                        </div>
                        <div>
                           <span>{{ item.product.desc1}}</span> <br>
                            <span><strong>Ksh {{ item.product.sell_price }}</strong></span>  
                        </div>
                        <div>
                            <div>
                                <span class="cart-qty"> {{ item.quantity }}</span>
                                <span><i class=" x icon"></i></span>
                                <span>{{ getItemTotal(item).toFixed(0) }}</span>
                            </div>
                        </div>
                    </div>
                    <!-- Bottom action buttons -->
                     <div class="cart-bot-nav-action-con">
                          <div class="cart-bot-nav-action">
                                <span @click="decrementQuantity(item)"><i class="bordered teal minus icon"></i></span>
                                <span class="cart-qty"> {{ item.quantity }}</span>
                                <span @click="incrementQuantity(item)"><i class="bordered inverted teal plus icon"></i></span>
                          </div>
                     </div>
                    <!-- Bottom action buttons -->
                    </div>
                 </div>
                 <!-- **************** Place order ****************/ -->
                  <div class="order-confirm-con">
                      <div class="order-confirm-items">
                          <div class="">
                              <span>Bill total items : <strong>({{ cartTotalLength }})</strong></span> <br>
                              <span>Tax: inclusive of tax</span><br>
                              <span>Grand Total: <span style="font-size:18px"><strong>Ksh {{ cartTotalPrice.toFixed(0)}}</strong></span></span>
                          </div>
                          <div></div>
                          <br>
                          <div><nuxt-link to="/checkout"><button class="fluid toggle ui teal button">Confirm Order</button></nuxt-link></div>
                      </div>
                  </div>
                 <!-- **************** Place order ****************/ -->
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
    name: 'Cart',
    data() {
        return {
            cart: {
                items: []
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
        console.warn(this.cart);
    },
    methods: {
        removeFromCart(item) {
            this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
        },
        clearCart(){
            this.cart = { items: [] }
            localStorage.setItem('cart', JSON.stringify(this.cart))
            this.$router.go(0)
        },

        getItemTotal(item) {
            return item.quantity * item.product.sell_price
        },
         decrementQuantity(item) {
            item.quantity -= 1

            if (item.quantity === 0) {
                this.removeFromCart(item)
            }

            this.updateCart()
        },
        incrementQuantity(item) {
            item.quantity += 1

            this.updateCart()
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
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
.index-top-spacer{
    margin-top: 110px;
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
.index-menu-items-con{
     background: white;
     margin-top: 10px;
     margin-bottom: 20px;
    border-radius: 5px;
    padding:10px 10px 0px 10px;
    box-shadow: 0 0 5px grey;
    -webkit-box-shadow: 0 0 5px rgb(78, 59, 59);
    -moz-box-shadow: 0 0 5px grey;
}
.index-menu-items{
    display: flex;
    justify-content: space-between; 
    align-items: center;
}
/***************************** NO PRODUCT CON */
.no-pro-img{
  max-width: 150px;
}
.no-pro-dis{
  font-family: 'Crimson Text', serif;
}

/***************** CART SUB ITEMS */

.cart-bot-nav-action-con{
    padding:5px 0px 5px 150px;
    background: #d9d9d9;
}
.cart-bot-nav-action{
   display: flex;
   align-items: center;
   gap: 20px;
}
/***          TOTAL BILL  */
.order-confirm-con{
    margin-top: 30px;
    margin-bottom: 50px;
    background: white;
    padding:10px 10px 10px 10px;
    box-shadow: 0 0 5px grey;
    -webkit-box-shadow: 0 0 5px grey;
    -moz-box-shadow: 0 0 5px grey;
}
</style>