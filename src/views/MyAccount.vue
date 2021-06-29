<template>
  <div id="myAccountPage">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">My Account</h1>
      </div>
      <div class="column is-12">
        <button @click="logout()" class="button is-danger">Log out</button>
      </div>
      <div class="column is-12">
        <h2 class="subtitle">My Orders</h2>
        <OrderSumary
          v-for="order in orders"
          v-bind:key="order.id"
          v-bind:order="order"
          />
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
import OrderSumary from '@/components/OrderSummary.vue'
export default {
  name: "MyAccount",
  components:{
    OrderSumary
  },
  data(){
    return {
      orders:[]
    }
  },
  mounted(){
    document.title="My Account | Ecommerce"
    this.getMyOrders()
  },
  methods:{
    logout:function(){
      axios.defaults.headers.common['Authorization'] =""
    
      localStorage.removeItem['token']
      localStorage.removeItem['username']
      localStorage.removeItem['user_id']

      this.$store.commit('removeToken')

      this.$router.push('/')
    },
    getMyOrders:async function(){
      this.$store.commit('setIsLoading',true)
      await axios.get('/api/v1/myOrders/')
            .then(response=>{
              this.orders = response.data
            }).catch(error=>{
              console.log(error)
            })

      this.$store.commit('setIsLoading',false)
    }
  }
};
</script>