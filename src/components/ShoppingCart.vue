<template>
  <div class="wrapper">
    <div class="shopping-cart-container" @click="openCloseSwitcher">
      <img
        src="../assets/imgs/new-shopping-cart.png"
        alt="shopping-cart"
        class="shopping-cart-image"
      />
      <span v-if="getSelectedProductsQuantity" class="selected-product-counter">
        {{ getSelectedProductsQuantity }}
      </span>
    </div>
    <div class="shop-cart-details" v-if="detailsOpenClose">
      <div v-if="!getSelectedProductsQuantity" class="shopping-cart-empty">
        No Items in the Shopping Cart.
      </div>
      <ul class="items-list">
        <li
          v-for="(product, index) in selectedProducts"
          class="single-item"
          :key="index"
        >
          <span class="arrow">> </span>
          {{ getProductShortTitle(product.title) }}
          <span class="item-quantity"
            >( {{ product.quantity }} item<span>{{
              getRightItemName(product.quantity)
            }}</span>
            )</span
          >
          <img
            src="../assets/imgs/delete.png"
            alt="delete-item"
            class="delete-button"
            @click="deleteProduct(product.id)"
          />
        </li>
      </ul>
      <div>
        <button @click="openCloseSwitcher" class="close-button">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["modelValue"],
  emits: ["update:modelValue"],
  data() {
    return {
      detailsOpenClose: false,
    };
  },
  computed: {
    selectedProducts() {
      return this.modelValue;
    },
    getSelectedProductsQuantity() {
      let selectedProductsQuantity = this.selectedProducts.reduce(
        (currentTotal, item) => {
          return item.quantity + currentTotal;
        },
        0
      );
      return selectedProductsQuantity;
    },
  },
  methods: {
    openCloseSwitcher() {
      this.detailsOpenClose = !this.detailsOpenClose;
    },
    getRightItemName(quantity) {
      return quantity > 1 ? "s" : "";
    },
    deleteProduct(id) {
      this.selectedProducts.forEach((element, index) => {
        if (element.id === id) {
          if (element.quantity > 1) {
            element.quantity -= 1;
          } else if (element.quantity === 1) {
            this.selectedProducts.splice(index, 1);
          }
        }
      });

      this.$emit("update:modelValue", this.selectedProducts);
    },
    getProductShortTitle(title) {
      return title.slice(0, 34) + " ..";
    },
  },
};
</script>

<style scoped>
.wrapper {
  position: relative;
}
.shopping-cart-container {
  width: 5rem;
  position: relative;
  cursor: pointer;
}

.shopping-cart-empty {
  margin: 1rem 0;
}

.shopping-cart-image {
  width: 70%;
}

.selected-product-counter {
  position: absolute;
  top: -0.6rem;
  right: 1rem;
  border-radius: 50%;
  color: firebrick;
  background-color: #fff;
  padding: 0.5rem;
  font-weight: bold;
  min-width: 2.5rem;
  text-align: center;
}

.item-quantity {
  color: #4caf50;
}
.shop-cart-details {
  position: absolute;
  z-index: 100;
  top: 2rem;
  right: 4rem;
  width: 33rem;
  background-color: rgba(227, 225, 225, 0.9);
  border: 1px solid #333333;
  border-radius: 0.3rem;
  padding: 5px 10px;
  color: #333;
}

.close-button {
  background-color: firebrick;
  border: none;
  color: white;
  padding: 10px 20px;
  text-align: center;
  font-size: 1rem;
  display: block;
  margin: 0.4rem auto;
  cursor: pointer;
}

.close-button:hover {
  color: firebrick;
  background-color: #fff;
  border: 1px solid firebrick;
}

.items-list {
  list-style: none;
  padding: 0;
}
.single-item {
  margin: 0.8rem;
}
.delete-button {
  width: 3%;
  margin-left: 0.4rem;
  cursor: pointer;
  position: relative;
  top: 5px;
}

.arrow {
  color: #4caf50;
  font-weight: bold;
  font-size: 1.2rem;
}
</style>
