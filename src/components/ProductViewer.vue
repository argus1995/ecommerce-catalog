<template>
  <div class="product-container">
    <div class="product-card">
      <div class="img-container">
        <img :src="currentProduct.image" :alt="currentProduct.title" />
      </div>
      <div class="desc-container">
        <h3>{{ currentProduct.title }}</h3>
        <div class="category-rating">
          <span class="category">{{ currentProduct.category }}</span>
          <div class="rating container">
            <span class="rating">2.9/5</span>
            <div class="rating-circle"></div>
            <div class="rating-circle"></div>
            <div class="rating-circle"></div>
            <div class="rating-circle-border"></div>
            <div class="rating-circle-border"></div>
          </div>
        </div>
        <div class="description">
          <p>{{ currentProduct.description }}</p>
        </div>
        <h3 class="price">${{ currentProduct.price }}</h3>
        <div class="button-container">
          <Button label="Buy Now" />
          <Button label="Next Product" @click="nextProduct" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { onMounted } from "vue";
import Button from "../components/Button.vue";

const products = ref([]);
const currentIndex = ref(0);

const currentProduct = computed(() => products.value[currentIndex.value] || {});
function nextProduct() {
  if (products.value.length > 0) {
    currentIndex.value = (currentIndex.value + 1) % products.value.length;
  }
}

onMounted(async () => {
  const res = await fetch("https://fakestoreapi.com/products/");
  products.value = await res.json();
});
</script>
