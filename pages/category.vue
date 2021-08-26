<template>
  <div class="ui container cat-main-con">
    <div v-if="$store.state.isLoading">
        <div class="ui segment">
            <div class="ui active loader"></div>
            <p>Loading ...</p>
        </div>
    </div>
    <div v-else>
     <div class="animate__animated animate__backInUp">
         <div class="cat-con" v-if="subcategory.length > 0">
        <div v-for="rs in subcategory" :key="rs.name">
            <nuxt-link :to="rs.get_absolute_url">
            <div class="cat-items">
                <div><img :src="rs.get_icon" class="cat-icon"></div>
                <div class="cat-txt">{{ rs.name}}</div>
            </div>
            </nuxt-link>
        </div>
        </div>
        <!-- No Products -->
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
         <!-- No Products -->
     </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      subcategory:[]
    }
  },
 
  mounted(){
    this.getsubcategory();
  },
 
  methods:{
    async getsubcategory(){
      const categorySlug = this.$route.params.category_slug
      this.$store.commit('setIsLoading', true)
      await this.$axios.$get(`/api/v1/categorylist/${categorySlug}/`)
       .then(response =>{
         this.subcategory = response;
       })
       this.$store.commit('setIsLoading', false)
    }

  }
}
</script>
<style scoped>

</style>