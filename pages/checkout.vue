<template>
    <div>
        <div class="ui container">
            <div class="checkout-con">
                <h1>Checkout</h1>

                <div class="" v-if="errors.length" style="background:orange">
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                </div>

                <div>
                    <form class="ui form">
                    <div class="">
                        <label>Full Name</label>
                        <input type="text"  v-model="consm_name" placeholder="i.e. John Doe">
                    </div>
                    <br>
                    <div class="">
                        <label>Phone Number</label>
                        <input type="text" v-model="phone" placeholder="i.e. 0795661900">
                    </div>
                    <br>
                    <div class="">
                        <label>Position: (Table no.)</label>
                        <input type="text" v-model="table_no" placeholder="i.e. 0024A">
                    </div>
                    <br>
                    <button  type="submit" class="fluid ui button" @click.prevent="submitForm">Place Order now</button>
                    </form>
                </div>
            </div>
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
            errors:[],
            consm_name:"",
            table_no:"",
            phone:"",
            bill_no:"0001245",
            trans_desc:"CASH SALE"
        }
    },
    mounted(){
        this.cart = this.$store.state.cart
    },
    methods:{
         async submitForm(){
            this.errors = []
            const items = []

            for (let i = 0; i < this.cart.items.length; i++) {
                const item = this.cart.items[i]
                // const obj = {
                //     product:  item.product.id,
                //     quantity: item.quantity,
                //     qty: item.quantity,
                //     unit_price: item.product.sell_price,
                //     avg_cost: (item.product.unit_cost * item.quantity) / 2,
                //     sell_price: item.product.sell_price * item.quantity,
                //     goods:    item.product.sell_price * item.quantity,
                //     vat: item.product.sell_price / 16,
                //     code:   item.product.code,
                //     table_no: this.table_no,
                //     bill_no:  this.bill_no,
                //     phone:    this.phone,
                //     consm_name: this.consm_name

                // }

                const obj = {
                    product: item.product.id,
                    qty: item.quantity,
                    goods: item.product.sell_price * item.quantity,
                    vat: item.product.sell_price / 16,
                    unit_price: item.product.sell_price,
                    avg_cost: (item.product.unit_cost * item.quantity) / 2,
                    code:   item.product.code,
                    table_no:this.table_no,
                    phone:    this.phone,
                    consm_name: this.consm_name,
                    trans_desc: this.trans_desc,
                    bill_no:  this.bill_no,
                }

                items.push(obj)
            }
            // let token = localStorage.getItem("token")
            // axios.defaults.headers.common["Authorization"] = "Token " + token
            const data = {
                'items': items,
                'tableno':this.table_no
            }
            console.warn(data)
           await this.$axios.$post('api/v1/checkout/', data)
                .then(() => {
                    this.$store.commit('clearCart');
                    this.$router.go();
                    // this.getApiProduct();
                    // this.$router.push('/dashboard')
                    
                })
                .catch(error => {
                    this.errors.push('Something went wrong try again later')
                    console.warn(error)
                })
        },
    }
}
</script>

<style scoped>
.checkout-con{
    margin-top: 110px;
}
</style>