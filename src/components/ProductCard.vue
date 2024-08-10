<template>
  <div class="col-md-4">
    <div class="card mb-4 box-shadow h-100">
      <img
        class="card-img-top"
        :src="product.image"
        alt="Product image"
        style="height: 200px; object-fit: cover"
      />
      <div class="card-body d-flex flex-column">
        <h5 class="card-title">{{ truncatedTitle }}</h5>
        <p class="card-text flex-grow-1">
          {{ truncatedDescription }}
          <span
            v-if="isDescriptionLong"
            class="text-primary cursor-pointer"
            @click="toggleDescription"
          >
            {{ showFullDescription ? "See less" : "See more" }}
          </span>
        </p>
        <div class="d-flex justify-content-between align-items-center mt-3">
          <small class="text-muted">Price: ${{ product.price }}</small>
          <div class="btn-group">
            <button
              :class="[
                'btn btn-sm',
                isInCart ? 'btn-danger' : 'btn-outline-secondary',
              ]"
              @click="toggleCart(product)"
            >
              <i class="fas fa-shopping-cart"></i>
            </button>
            <button
              :class="[
                'btn btn-sm',
                isInFavorites ? 'btn-danger' : 'btn-outline-secondary',
              ]"
              @click="toggleFavorites(product)"
            >
              <i class="fas fa-heart"></i>
            </button>
          </div>
        </div>
        <div class="rating mt-2">
          <span
            v-for="star in 5"
            :key="star"
            class="fa fa-star"
            :class="{
              'text-warning': star <= product.rating,
              'text-muted': star > product.rating,
            }"
          ></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["product"],
  data() {
    return {
      showFullDescription: false,
      isInCart: false,
      isInFavorites: false,
    };
  },
  computed: {
    truncatedTitle() {
      const title = this.product.title || "";
      return title.length <= 19 ? title : title.slice(0, 19);
    },
    truncatedDescription() {
      const description = this.product.description || "";
      return this.showFullDescription || description.length <= 100
        ? description
        : description.slice(0, 100) + "...";
    },
    isDescriptionLong() {
      return this.product.description && this.product.description.length > 100;
    },
  },
  methods: {
    toggleCart(product) {
      let cartItems = JSON.parse(localStorage.getItem("cartItems")) || [];

      if (this.isInCart) {
        // Remove item from cart
        cartItems = cartItems.filter((item) => item.id !== product.id);
        this.isInCart = false;
      } else {
        // Add item to cart
        product.quantity = 1;
        cartItems.push(product);
        this.isInCart = true;
      }

      localStorage.setItem("cartItems", JSON.stringify(cartItems));
      this.$root.$emit("update-cart", cartItems.length);
    },
    toggleFavorites(product) {
      let favorites = JSON.parse(localStorage.getItem("favorites")) || [];

      if (this.isInFavorites) {
        // Remove item from favorites
        favorites = favorites.filter((item) => item.id !== product.id);
        this.isInFavorites = false;
      } else {
        // Add item to favorites
        favorites.push(product);
        this.isInFavorites = true;
      }

      localStorage.setItem("favorites", JSON.stringify(favorites));
      this.$root.$emit("update-favorites", favorites.length);
    },
    toggleDescription() {
      this.showFullDescription = !this.showFullDescription;
    },
  },
  created() {
    let cartItems = JSON.parse(localStorage.getItem("cartItems")) || [];
    this.isInCart = cartItems.some((item) => item.id === this.product.id);

    let favorites = JSON.parse(localStorage.getItem("favorites")) || [];
    this.isInFavorites = favorites.some((item) => item.id === this.product.id);
  },
};
</script>
