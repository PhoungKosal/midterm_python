<template>
  <div class="sticky-top">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark bg-gray-900">
      <div class="container-fluid">
        <a class="navbar-brand me-4" href="/">SS20 LOGO</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav me-auto"></ul>
          <div class="d-flex flex-nowrap align-items-center">
            <router-link
              to="/cart"
              class="nav-link position-relative me-3"
              @click.native="showLoading"
            >
              <i class="fas fa-cart-plus"></i>

              <span
                class="badge rounded-pill bg-warning text-dark position-absolute"
                style="top: -5px; right: -10px"
              >
                {{ cartCount }}
              </span>
            </router-link>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import $ from "jquery";
import "gasparesganga-jquery-loading-overlay/dist/loadingoverlay.min.js";

export default {
  data() {
    return {
      cartCount: 0,
      favoriteCount: 0,
      darkMode: false, // Default mode
      isLoading: false, // Loading state
    };
  },
  created() {
    // Listen for cart and favorite updates
    this.$root.$on("update-cart", this.updateCartCount);
    this.$root.$on("update-favorites", this.updateFavoriteCount);

    // Load initial cart count from localStorage
    this.loadInitialCartCount();

    // Load dark mode preference from localStorage
    this.darkMode = JSON.parse(localStorage.getItem("darkMode")) || false;
    this.applyDarkMode();
  },
  methods: {
    updateCartCount(count) {
      this.cartCount = count;
    },
    updateFavoriteCount(count) {
      this.favoriteCount = count;
    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
      localStorage.setItem("darkMode", JSON.stringify(this.darkMode));
      this.applyDarkMode();
    },
    applyDarkMode() {
      if (this.darkMode) {
        document.body.classList.add("bg-dark", "text-light");
      } else {
        document.body.classList.remove("bg-dark", "text-light");
      }
    },
    logout() {
      // Implement logout functionality
      alert("Logged out");
    },
    loadInitialCartCount() {
      const cart = JSON.parse(localStorage.getItem("cartItems")) || [];
      this.cartCount = cart.length;
    },
    showLoading() {
      this.isLoading = true;
      $.LoadingOverlay("show");

      // Hide loading overlay after 3 seconds
      setTimeout(() => {
        $.LoadingOverlay("hide");
        this.isLoading = false;
      }, 500); // Adjust this duration as needed
    },
  },
};
</script>
