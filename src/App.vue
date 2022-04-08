<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col sm="4">
          <h2>Добавить продукт</h2>
          <addProduct @addProduct="addProduct" />
          <h2>настройки</h2>
          <v-switch hide-details flat color="blue" label="показывать бд" v-model="bdShow"/>
        </v-col>
        <v-col sm="8">
          <productDB :products="products" @deleteProduct="deleteProduct" v-if="bdShow"/>
          <myProduct :products="products" />
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import addProduct from "@/components/addProduct.vue";
import productDB from "./components/productDB.vue";
import myProduct from "./components/myProduct.vue";
export default {
  components: {
    addProduct,
    productDB,
    myProduct
  },
  name: "App",
  data: () => ({
    products: [],
    bdShow:true
  }),
  methods: {
    deleteProduct(i) {
      this.products.splice(i, 1);
      this.saveProducts();
    },
    saveProducts() {
      localStorage.products = JSON.stringify(this.products);
    },
    addProduct(product) {
      this.products.push({ ...product });
      this.saveProducts();
    },
  },
  mounted() {
    vm = this;
    localStorage.products = localStorage.products || "[]";
    this.products = JSON.parse(localStorage.products);
  },
};
</script>
<style scss>
</style>