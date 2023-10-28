<template>
  <div :class="productCategory">
    <button @click="fetchNextProduct">Next Product</button>
    <div v-if="product" class="product-card">
      <h2>{{ product.title }}</h2>
      <p>{{ product.description }}</p>
      <p>Price: ${{ product.price }}</p>
      <p>Category: {{ product.category }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1,
      product: null,
    };
  },
  computed: {
    productCategory() {
      return this.product && (this.product.category === "men's clothing" || this.product.category === "women's clothing")
        ? `page-${this.product.category.replace(/ /g, '-').toLowerCase()}`
        : 'page-unavailable-product';
    },
  },
  methods: {
    fetchNextProduct() {
      this.index = (this.index % 20) + 1;
      axios
        .get(`https://fakestoreapi.com/products/${this.index}`)
        .then((response) => {
          const data = response.data;
          if (data.category === "men's clothing" || data.category === "women's clothing") {
            this.product = data;
          } else {
            this.product = null;
          }
        });
    },
  },
};
</script>

<style>
/* CSS untuk halaman produk kategori pria */
.page-men-s-clothing {
  background-color: #a0e7e0;
  color: #333;
}

/* CSS untuk halaman produk kategori wanita */
.page-women-s-clothing {
  background-color: #f7a5c2;
  color: #333;
}

/* CSS untuk halaman produk yang tidak tersedia */
.page-unavailable-product {
  background-color: #f0f0f0;
  color: #777;
}

/* CSS untuk kartu produk */
.product-card {
  border: 1px solid #ddd;
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}




</style>
