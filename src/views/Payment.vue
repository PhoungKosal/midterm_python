<template>
  <div class="container py-8 mt-5 p-5">
    <div class="row g-4">
      <!-- Product Information Section -->
      <div class="col-lg-6">
        <PaymentSummary
          :cart-items="cartItems"
          :shipping-fee="shippingFee"
          :tax-rate="taxRate"
        />
      </div>

      <!-- Payment Form Section -->
      <div class="col-lg-6">
        <PaymentForm
          @submitPayment="handleSubmit"
          @goBackToCart="goBackToCart"
          :loading="loading"
        />
      </div>
    </div>

    <!-- Loading overlay -->
    <div v-if="loading" class="loading-overlay">
      <div class="spinner"></div>
    </div>

    <!-- Success Popup -->
    <div v-if="showSuccess" class="popup-overlay">
      <div class="popup-content">
        <div class="checkmark-wrapper">
          <svg
            class="checkmark"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 52 52"
          >
            <circle
              class="checkmark-circle"
              cx="26"
              cy="26"
              r="25"
              fill="none"
            />
            <path class="checkmark-check" fill="none" d="M14 27l7 7 16-16" />
          </svg>
        </div>
        <h3>Your order was successfully placed!</h3>
        <label>Thank you</label>
      </div>
    </div>
  </div>
</template>

<script>
import PaymentSummary from "../components/PaymentSummary.vue";
import PaymentForm from "../components/PaymentForm.vue";
import axios from "axios";

export default {
  components: {
    PaymentSummary,
    PaymentForm,
  },
  data() {
    return {
      cartItems: [],
      shippingFee: 5.0, // shipping fee
      taxRate: 0.1, // rate (10%)
      showSuccess: false,
      loading: false, // loading state
    };
  },
  computed: {
    subtotal() {
      return this.cartItems.reduce(
        (sum, item) => sum + item.price * item.quantity,
        0
      );
    },
    tax() {
      return this.subtotal * this.taxRate;
    },
    orderTotal() {
      return this.subtotal + this.shippingFee + this.tax;
    },
  },
  created() {
    this.loadCart();
  },
  methods: {
    loadCart() {
      const cart = localStorage.getItem("cartItems");
      this.cartItems = cart ? JSON.parse(cart) : [];
    },
    async handleSubmit(paymentInfo) {
      this.loading = true; // Start loading

      // Simulate a delay for the loading effect
      setTimeout(async () => {
        // Show success popup
        this.showSuccess = true;
        this.loading = false; // Stop loading

        // Send message to Telegram bot
        await this.sendTelegramMessage(paymentInfo);

        // Clear cart and redirect after a delay
        setTimeout(() => {
          this.clearCart();
          this.$router.push("/");
          window.location.href = "/"; // Redirect to home page
          window.location.reload(); // Force reload to update cart icon
        }, 3000); // Redirect to home page after 3 seconds
      }, 1000); // Simulated delay
    },
    async sendTelegramMessage(paymentInfo) {
      const message =
        `🛎 *New Order Confirmation* 🛎\n\n` +
        `👤 *Name*: ${paymentInfo.username}\n` +
        `📞 *Phone*: ${paymentInfo.contact}\n` +
        `📧 *Email*: ${paymentInfo.email}\n` +
        `🏠 *Address*: ${paymentInfo.address}\n\n` +
        `--------------------------------------\n` +
        `📅 *Booking Date*: ${new Date().toLocaleDateString()}\n` +
        `--------------------------------------\n\n` +
        `📦 *Items*:\n\n` +
        this.cartItems
          .map(
            (item) =>
              `${item.title} (Quantity: ${item.quantity}, Price: $${item.price})`
          )
          .join("\n") +
        `\n\n` +
        `💵 *Total Price*: $${this.orderTotal.toFixed(2)}\n\n` +
        `🎉 Thank you for your support! 🎉`;

      const TELEGRAM_BOT_TOKEN =
        "7348435170:AAHFL_032rqkiXXGg8s-H68zZPuE9k-V4w0";
      const TELEGRAM_CHAT_ID = "@ss20_flask";

      const url = `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`;

      await axios.post(url, {
        chat_id: TELEGRAM_CHAT_ID,
        text: message,
        parse_mode: "Markdown",
      });
    },
    clearCart() {
      localStorage.removeItem("cartItems");
      this.cartItems = [];
    },
    goBackToCart() {
      this.$router.push("/cart");
    },
    closePopup() {
      this.showSuccess = false;
    },
  },
};
</script>
