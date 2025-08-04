<template>
  <div
    :style="{ background: currentStyle.background }"
    class="product-container"
  >
    <Spinner v-if="isLoading" />
    <div v-if="currentStyle.show" class="product-card">
      <div class="img-container">
        <img :src="currentProduct.image" alt="Product Image" />
      </div>
      <div class="desc-container">
        <h3 :style="{ color: currentStyle.color }">
          {{ currentProduct.title }}
        </h3>
        <div class="category-rating">
          <span class="category">{{ currentProduct.category }}</span>
          <div class="rating-container">
            <span class="rating">2.9/5</span>
            <div
              class="rating-circle"
              :style="{
                background: currentStyle.color,
              }"
            ></div>
            <div
              class="rating-circle"
              :style="{
                background: currentStyle.color,
              }"
            ></div>
            <div
              class="rating-circle"
              :style="{
                background: currentStyle.color,
              }"
            ></div>
            <div
              class="rating-circle-border"
              :style="{
                border: currentStyle.border,
              }"
            ></div>
            <div
              class="rating-circle-border"
              :style="{
                border: currentStyle.border,
              }"
            ></div>
          </div>
        </div>
        <div class="description">
          <p>{{ currentProduct.description }}</p>
        </div>
        <h3 class="price" :style="{ color: currentStyle.color }">
          ${{ currentProduct.price }}
        </h3>
        <div class="button-container">
          <Button label="Buy Now" :style="{ background: currentStyle.color }" />
          <Button
            label="Next Product"
            @click="nextProduct"
            :style="{ border: currentStyle.border, color: currentStyle.color }"
          />
        </div>
      </div>
    </div>
    <div v-else class="product-card-unavailable">
      <p>This product is unavailable to show</p>
      <Button
        label="Next Product"
        @click="nextProduct"
        :style="{ border: currentStyle.border, color: currentStyle.color }"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { onMounted } from "vue";
import Button from "../components/Button.vue";
import Spinner from "./Spinner.vue";

const products = ref([]);
const currentIndex = ref(0);
const isLoading = ref(true);

const currentProduct = computed(() => products.value[currentIndex.value] || {});

const currentStyle = computed(() => {
  const category = currentProduct.value.category;
  if (category === "men's clothing") {
    return {
      background:
        "linear-gradient(to bottom, var(--bg-men) 0%, var(--bg-men) 60%, var(--bg-main) 60%, var(--bg-main) 100%)",
      color: "var(--color-men)",
      border: "2px solid var(--color-men)",
      show: true,
    };
  } else if (category === "women's clothing") {
    return {
      background:
        "linear-gradient(to bottom, var(--bg-women) 0%, var(--bg-women) 60%, var(--bg-main) 60%, var(--bg-main) 100%)",
      color: "var(--color-women)",
      border: "2px solid var(--color-women)",
      show: true,
    };
  } else {
    return {
      background:
        "linear-gradient(to bottom, var(--bg-grey) 0%, var(--bg-grey) 60%, var(--bg-main) 60%, var(--bg-grey) 100%)",
      color: "var(--text-grey)",
      border: "2px solid var(--text-grey)",
      show: false,
    };
  }
});

function nextProduct() {
  if (products.value.length > 0) {
    currentIndex.value = (currentIndex.value + 1) % products.value.length;
  }
}

onMounted(async () => {
  isLoading.value = true;
  try {
    const res = await fetch("https://fakestoreapi.com/products");
    const data = await res.json();
    products.value = data;
  } catch (e) {
    console.error("Failed to load products", e);
  } finally {
    isLoading.value = false;
  }
});
</script>

<style scoped></style>
