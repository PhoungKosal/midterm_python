<template>
  <section class="rounded bg-white p-4 shadow-lg">
    <h2 class="h5 text-dark mb-4">Order Summary</h2>
    <table class="table table-bordered">
      <thead class="thead-light">
        <tr>
          <th scope="col">No</th>
          <th scope="col">Name</th>
          <th scope="col">Price</th>
          <th scope="col">Qty</th>
          <th scope="col">Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in cartItems" :key="index">
          <th scope="row">{{ index + 1 }}</th>
          <td>{{ item.title }}</td>
          <td>${{ item.price.toFixed(2) }}</td>
          <td>{{ item.quantity }}</td>
          <td>${{ (item.price * item.quantity).toFixed(2) }}</td>
        </tr>
      </tbody>
    </table>
    <dl class="mt-4">
      <div class="d-flex justify-content-between">
        <dt class="small text-muted">Subtotal</dt>
        <dd class="small font-weight-bold text-dark">
          ${{ subtotal.toFixed(2) }}
        </dd>
      </div>
      <div class="d-flex justify-content-between border-top pt-2">
        <dt class="d-flex align-items-center small text-muted">
          <span>Shipping estimate</span>
        </dt>
        <dd class="small font-weight-bold text-dark">
          ${{ shippingFee.toFixed(2) }}
        </dd>
      </div>
      <div class="d-flex justify-content-between border-top pt-2">
        <dt class="d-flex align-items-center small text-muted">
          <span>Tax estimate</span>
        </dt>
        <dd class="small font-weight-bold text-dark">${{ tax.toFixed(2) }}</dd>
      </div>
      <div
        class="d-flex justify-content-between border-top pt-2 font-weight-bold text-dark"
      >
        <dt>Order total</dt>
        <dd>${{ orderTotal.toFixed(2) }}</dd>
      </div>
    </dl>
  </section>
</template>

<script>
export default {
  props: {
    cartItems: {
      type: Array,
      required: true,
    },
    shippingFee: {
      type: Number,
      required: true,
    },
    taxRate: {
      type: Number,
      required: true,
    },
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
};
</script>
