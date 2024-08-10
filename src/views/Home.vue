<template>
  <div>
    <div v-if="error" class="error">Error fetching products: {{ error }}</div>
    <ProductTable v-if="!loading && !error" :products="products" />
  </div>
</template>

<script>
import axios from "axios";
import ProductTable from "../components/ProductTable.vue";
import $ from "jquery";

export default {
  name: "App",
  components: {
    ProductTable,
  },
  data() {
    return {
      products: [],
      loading: false,
      error: null,
    };
  },
  created() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      // this.loading = true;
      this.error = null;

      try {
        $.LoadingOverlay("show"); // Show loading overlay
        const response = await axios.get("https://fakestoreapi.com/products");
        this.products = response.data;
      } catch (error) {
        this.error = "Failed to load products.";
        console.error("Error fetching products:", error);
      } finally {
        this.loading = false;
        setTimeout(() => {
          $.LoadingOverlay("hide"); // Hide loading overlay
        }, 1000); // Hide after 3 seconds
      }
    },
  },
};
</script>
