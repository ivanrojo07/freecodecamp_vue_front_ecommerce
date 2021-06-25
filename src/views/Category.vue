<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-site-2 has-text-centered">{{category.name}}</h2>
      </div>
      <ProductBox
        v-for="product in category.products"
        :key="product.id"
        :product="product"></ProductBox>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";
import ProductBox from '@/components/ProductBox'

export default {
  name: "Category",
  data() {
    return {
      category: {
        products: [],
      },
    };
  },
  components:{
    ProductBox
  },
  mounted() {
    this.getCategory();
  },
  watch:{
    $route(to){
      if(to.name === 'Category'){
        this.getCategory()
      }
    }
  },
  methods: {
    getCategory: async function () {
      this.$store.commit("setIsLoading", true);
      const categorySlug = this.$route.params.category_slug;

      await axios
        .get(`/api/v1/categories/${categorySlug}/`)
        .then((response) => {
          this.category = response.data;
        })
        .catch((error) => {
          console.log(error);
          toast({
            message: "Something went wrong, Please try Again.",
            type: "is-danger",
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
            position: "bottom-right",
          });
        });
      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>