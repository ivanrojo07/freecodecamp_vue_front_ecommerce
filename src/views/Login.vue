<template>
  <div id="loginPage">
    <div class="columns">
      <div class="column is-4 is-offset-4">
        <h1 class="title">Log In</h1>
        <form @submit.prevent="submitForm">
          <div class="field">
            <label for="username">Username</label>
            <div class="control">
              <input
                type="text"
                class="input"
                id="username"
                v-model="username"
              />
            </div>
          </div>
          <div class="field">
            <label for="password">Password</label>
            <div class="control">
              <input
                type="password"
                class="input"
                id="password"
                v-model="password"
              />
            </div>
          </div>
          <div class="notification is-danger" v-if="errors.length">
            <p v-for="error in errors" :key="error">{{ error }}</p>
          </div>

          <div class="field">
            <div class="control">
              <button class="button is-dark" type="submit">Log in</button>
            </div>
          </div>
          <hr />
          Or <router-link to="/signup">click here</router-link> to Signup!
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
      errors: [],
    };
  },
  mounted(){
    document.title="Log In | Ecommerce"
  },
  methods: {
    submitForm: async function () {
      axios.defaults.headers.common['Authorization']=""
      localStorage.removeItem["token"]
      const formData = {
        username:this.username,
        password:this.password
      }

      await axios.post('/api/v1/token/login/',formData)
                .then(response=>{
                  console.log(response.data)
                  const token = response.data.auth_token
                  this.$store.commit('setToken',token)

                  axios.defaults.headers.common['Authorization']=`Token ${token}`
                  localStorage.setItem("token",token)

                  const toPath = this.$route.query.to || '/cart'

                  this.$router.push(toPath)
                })
                .catch(error=>{
                  if(error.response){
                    for(const property in error.response.data){
                      this.errors.push(`${property}: ${error.response.data[property]}`)
                    }
                  }
                  else{
                    this.errors.push("Sumething went wrong. Please try again")
                    console.log(JSON.stringify(error))
                  }
                })
    },
  },
};
</script>