<template>
  <div class="product-img-div">
    <img class="product-img" :src="imgUrl" :alt="title" />
  </div>
  <div class="product-name">
    <b>{{ getShortProductTitle }}</b>
  </div>
  <div class="product-description">{{ getShortDescription }}</div>
  <div>
    <button @click="addToCart" class="add-to-cart">Add to Cart</button>
  </div>
</template>

<script>
export default {
  props: ["title", "description", "imgUrl", "id"],
  emits: ["add-to-cart"],
  computed: {
    getShortDescription() {
      return this.description.slice(0, 50) + "...";
    },
    getShortProductTitle() {
      return this.title.slice(0, 22);
    },
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", { id: this.id, title: this.title });
    },
  },
};
</script>

<style scoped>
.product-name {
  text-align: center;
  margin-bottom: 1rem;
  min-height: 2rem;
}
.product-description {
  font-size: 0.8rem;
  margin-bottom: 1rem;
  min-height: 3rem;
}
.product-img-div {
  margin-bottom: 2rem;
  height: 15rem;
}
.product-img {
  width: 65%;
  display: block;
  margin: 0 auto;
}
.add-to-cart {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  font-size: 1rem;
  display: block;
  margin: 0 auto;
  cursor: pointer;
}
.add-to-cart:hover {
  color: #4caf50;
  background-color: azure;
  border: 1px solid #4caf50;
}
</style>
