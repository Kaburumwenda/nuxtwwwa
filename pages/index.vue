<template>
  <div class="ui container cat-main-con">
    <div class="cat-con">
      <div v-for="rs in category" :key="rs.name">
        <nuxt-link :to="rs.get_absolute_url">
           <div class="cat-items">
             <div><img :src="rs.get_icon" class="cat-icon"></div>
             <div class="cat-txt">{{ rs.name}}</div>
           </div>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      category:[]
    }
  },
  created(){
   this.getCategoryApi();
   this.getCategory();
  },
  mounted(){
    this.getCategory();
    
  },

  methods:{
    async getCategory(){
      this.$store.commit('setIsLoading', true)
      await this.$axios.$get('/api/v1/categorylist/')
       .then(response =>{
         this.category = response;
        //  localStorage.setItem("category", JSON.stringify(response));
       })
       this.$store.commit('setIsLoading', false)
    },
    getCategoryApi(){
      // var data = JSON.parse(localStorage.getItem("category"));
      // this.category = data;
    }

  }
}
</script>
