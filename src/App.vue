<template>
  <div id="app">
    <Header />
    <router-view />
    <!-- Render Footer only on Home page -->
    <Footer v-if="isHomePage" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
export default {
  name: "App",
  components: {
    Header,
  },
  computed: {
    // Check if the current route is the Home route
    isHomePage() {
      return this.$route.name === "Home";
    },
  },
};
</script>

<style>
/* Add global styles here */
</style>
