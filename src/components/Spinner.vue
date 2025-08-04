<template>
  <div class="spinner"></div>
</template>

<style scoped>
.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #ccc;
  border-top-color: #002772;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
  margin: auto;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
