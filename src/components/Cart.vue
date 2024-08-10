<template>
  <div class="container mt-4">
    <h2 class="mb-4 text-center">Your Cart</h2>
    <div v-if="cartItems.length === 0" class="text-center">
      <p class="lead">Your cart is empty</p>
    </div>
    <div v-else>
      <div class="card">
        <div class="card-body">
          <ul class="list-group list-group-flush">
            <li
              v-for="item in cartItems"
              :key="item.id"
              class="list-group-item d-flex align-items-center mb-3"
            >
              <img
                :src="item.image"
                alt="Product Image"
                class="img-thumbnail mr-4 p-2"
                style="width: 120px; height: auto; margin-right: 20px"
              />
              <div class="flex-grow-1">
                <h5 class="mb-2">{{ item.title }}</h5>
                <p class="text-muted mb-2">{{ item.description }}</p>
                <div class="d-flex align-items-center justify-content-between">
                  <div class="d-flex align-items-center">
                    <button
                      @click="decrementQuantity(item)"
                      class="btn btn-outline-secondary btn-sm mx-1"
                    >
                      -
                    </button>
                    <span class="mx-3">{{ item.quantity }}</span>
                    <button
                      @click="incrementQuantity(item)"
                      class="btn btn-outline-secondary btn-sm mx-1"
                    >
                      +
                    </button>
                  </div>
                  <p class="mb-0">
                    <strong
                      >${{ (item.price * item.quantity).toFixed(2) }}</strong
                    >
                  </p>
                  <button
                    @click="removeFromCart(item.id)"
                    class="btn btn-sm btn-danger"
                  >
                    Remove
                  </button>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div class="card mt-4">
        <div class="card-body text-right">
          <h4>Total: ${{ total }}</h4>
          <button @click="proceedToPayment" class="btn btn-primary">
            Proceed to Payment
          </button>
        </div>
      </div>
    </div>

    <!-- Loading overlay -->
    <div
      v-if="isLoading"
      class="loading-overlay d-flex align-items-center justify-content-center"
    >
      <div class="spinner-border text-primary" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery";
import "gasparesganga-jquery-loading-overlay/dist/loadingoverlay.min.js"; // Import LoadingOverlay JS

export default {
  data() {
    return {
      cartItems: [],
      isLoading: false, // Add loading state
    };
  },
  computed: {
    total() {
      return this.cartItems
        .reduce((sum, item) => sum + item.price * item.quantity, 0)
        .toFixed(2);
    },
  },
  created() {
    this.isLoading = true;
    $.LoadingOverlay("show");

    this.loadCart();

    // Simulate a delay to show the loading overlay
    setTimeout(() => {
      $.LoadingOverlay("hide");
      this.isLoading = false;
    }, 800); // Adjust this duration as needed

    this.$root.$on("update-cart", this.loadCart);
  },
  methods: {
    loadCart() {
      const cart = localStorage.getItem("cartItems");
      this.cartItems = cart ? JSON.parse(cart) : [];
    },
    saveCart() {
      localStorage.setItem("cartItems", JSON.stringify(this.cartItems));
    },
    removeFromCart(id) {
      this.cartItems = this.cartItems.filter((item) => item.id !== id);
      this.saveCart();
      this.$root.$emit("update-cart", this.cartItems.length);
    },
    incrementQuantity(item) {
      item.quantity++;
      this.saveCart();
      this.$root.$emit("update-cart", this.cartItems.length);
    },
    decrementQuantity(item) {
      if (item.quantity > 1) {
        item.quantity--;
        this.saveCart();
        this.$root.$emit("update-cart", this.cartItems.length);
      }
    },
    proceedToPayment() {
      this.isLoading = true;
      $.LoadingOverlay("show");

      // Simulate loading time before navigating
      setTimeout(() => {
        $.LoadingOverlay("hide");
        this.isLoading = false;
        this.$router.push("/payment"); // Navigate programmatically
      }, 800); // Adjust this duration as needed
    },
  },
  beforeDestroy() {
    this.$root.$off("update-cart", this.loadCart);
  },
};
</script>
