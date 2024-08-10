<template>
  <section class="rounded bg-white p-4 shadow-lg">
    <h2 class="text-lg font-medium text-gray-900 mb-4">Payment Information</h2>
    <form @submit.prevent="submitPayment">
      <!-- Username -->
      <div class="mb-4">
        <label for="username" class="form-label">Username</label>
        <input
          type="text"
          id="username"
          v-model="username"
          class="form-control"
          required
        />
      </div>

      <!-- Contact Number -->
      <div class="mb-4">
        <label for="contact" class="form-label">Contact Number</label>
        <input
          type="text"
          id="contact"
          v-model="contact"
          class="form-control"
          required
        />
      </div>

      <!-- Email -->
      <div class="mb-4">
        <label for="email" class="form-label">Email</label>
        <input
          type="email"
          id="email"
          v-model="email"
          class="form-control"
          required
        />
      </div>

      <!-- Address -->
      <div class="mb-4">
        <label for="address" class="form-label">Address</label>
        <input
          type="text"
          id="address"
          v-model="address"
          class="form-control"
          required
        />
      </div>

      <div class="d-flex justify-content-between">
        <button type="button" class="btn btn-secondary" @click="goBackToCart">
          Back to Cart
        </button>
        <button type="submit" class="btn btn-primary" :disabled="loading">
          <span v-if="loading">
            <i class="fas fa-spinner fa-spin"></i> Processing...
          </span>
          <span v-else> Pay Now </span>
        </button>
      </div>
    </form>
  </section>
</template>
<template>
  <section class="rounded bg-white p-4 shadow-lg">
    <h2 class="text-lg font-medium text-gray-900 mb-4">Payment Information</h2>
    <form @submit.prevent="submitPayment">
      <!-- Username -->
      <div class="mb-4">
        <label for="username" class="form-label">Username</label>
        <input
          type="text"
          id="username"
          v-model="username"
          class="form-control"
          required
        />
      </div>

      <!-- Contact Number -->
      <div class="mb-4">
        <label for="contact" class="form-label">Contact Number</label>
        <input
          type="text"
          id="contact"
          v-model="contact"
          class="form-control"
          required
        />
      </div>

      <!-- Email -->
      <div class="mb-4">
        <label for="email" class="form-label">Email</label>
        <input
          type="email"
          id="email"
          v-model="email"
          class="form-control"
          required
        />
      </div>

      <!-- Address -->
      <div class="mb-4">
        <label for="address" class="form-label">Address</label>
        <input
          type="text"
          id="address"
          v-model="address"
          class="form-control"
          required
        />
      </div>

      <div class="d-flex justify-content-between">
        <button type="button" class="btn btn-secondary" @click="goBackToCart">
          Back to Cart
        </button>
        <button type="submit" class="btn btn-primary" :disabled="loading">
          <span v-if="loading">
            <i class="fas fa-spinner fa-spin"></i> Processing...
          </span>
          <span v-else> Pay Now </span>
        </button>
      </div>
    </form>
  </section>
</template>

<script>
export default {
  props: {
    loading: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      username: "",
      contact: "",
      email: "",
      address: "",
    };
  },
  methods: {
    submitPayment() {
      const paymentInfo = {
        username: this.username, // Updated field name here
        contact: this.contact,
        email: this.email,
        address: this.address,
      };
      this.$emit("submitPayment", paymentInfo);
    },
    goBackToCart() {
      this.$emit("goBackToCart");
    },
  },
};
</script>
