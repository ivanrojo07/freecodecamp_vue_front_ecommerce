<template>
  <div class="home">

    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <p class="title mb-6">
          Welcome to Ecommerce
        </p>
        <p class="subtitle">
          For Ivan
        </p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-site-2 has-text-centered">Latest Products</h2>
      </div>
      <ProductBox
        v-for="product in latestProducts"
        :key="product.id"
        :product="product"></ProductBox>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'

export default {
  name: 'Home',
  data(){
    return {
      latestProducts:[]
    }
  },
  components: {
    ProductBox
  },
  mounted(){
    this.getLatestProducts()
    
  },
  methods:{
    getLatestProducts:function(){
      axios.get('/api/v1/latest-products/').then(response=>{
        this.latestProducts = response.data
      }).catch(error=>{
        console.log(error)
      })
    }
  }
}
</script>

<style scoped>
  .image{
    margin-top: -1.25rem;
    margin-left: -1.25rem;
    margin-right: -1.25rem;
  }
</style>