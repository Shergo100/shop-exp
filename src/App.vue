<template>
  <header>
    <div class="header-middle-page">
      <h1>Shop Experience</h1>
      <shopping-cart v-model="selectedProducts"></shopping-cart>
    </div>
  </header>
  <main>
    <div v-if="isLoading" class="loader"></div>
    <p v-else-if="!isLoading && error">
      {{ error }}
    </p>
    <div v-else-if="!isLoading && (!products || products.length === 0)">
      No Products found!
    </div>
    <div class="product-container" v-else>
      <div v-for="product in products" :key="product.id" class="single-product">
        <single-product
          :title="product.title"
          :description="product.description"
          :imgUrl="product.image"
          :id="product.id"
          @addToCart="addToCart"
        ></single-product>
      </div>
    </div>
  </main>
</template>

<script>
import SingleProduct from "./components/SingleProduct.vue";
import ShoppingCart from "./components/ShoppingCart.vue";

export default {
  name: "App",
  components: {
    SingleProduct,
    ShoppingCart,
  },
  data() {
    return {
      products: [],
      selectedProducts: [],
      isLoading: false,
      error: null,
    };
  },
  created() {
    this.loadProducts();
  },
  methods: {
    loadProducts() {
      this.isLoading = true;
      this.error = null;
      fetch("https://fakestoreapi.com/products?limit=20")
        .then((res) => {
          if (res.ok) {
            return res.json();
          } else {
            throw new Error(
              "Failed to fetch Products, please try again later!"
            );
          }
        })
        .then((data) => {
          this.isLoading = false;
          this.products = data;
        })
        .catch((err) => {
          this.isLoading = false;
          this.error = err.message;
        });
    },
    addToCart(data) {
      let selectedProductIndex = null;
      const selectedProductExistence = this.selectedProducts.find(
        (item, index) => {
          if (item.id === data.id) {
            selectedProductIndex = index;
            return true;
          }
        }
      );

      if (selectedProductExistence) {
        this.selectedProducts[selectedProductIndex]["quantity"] += 1;
      } else {
        this.selectedProducts.push({
          id: data.id,
          title: data.title,
          quantity: 1,
        });
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  box-sizing: border-box;
}

header {
  background-color: firebrick;
  color: #fff;
  min-height: 10vh;
  padding: 5px 0;
}
.header-middle-page {
  width: 90%;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  padding: 0.7rem 4rem 0.7rem 1rem;
}
.header-middle-page h1 {
  vertical-align: middle;
  font-family: cursive;
}
main {
  width: 90%;
  margin: 0 auto;
}

.product-container {
  margin-top: 1%;
  display: flex;
  flex-wrap: wrap;
}

.single-product {
  width: 15rem;
  border: 1px solid #aaa;
  height: 30rem;
  margin: 0 1% 2%;
  padding: 1% 1% 0;
  overflow: hidden;
}

.loader {
  border: 10px solid #f3f3f3;
  border-radius: 50%;
  border-top: 10px solid #4caf50;
  border-bottom: 10px solid #4caf50;
  width: 75px;
  height: 75px;
  -webkit-animation: spin 2s linear infinite;
  animation: spin 2s linear infinite;
  position: absolute;
  top: 50%;
  left: 50%;
}

@-webkit-keyframes spin {
  from {
    -webkit-transform: rotate(0deg);
  }
  to {
    -webkit-transform: rotate(360deg);
  }
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
