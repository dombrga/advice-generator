<template>
  <p class="advice__text">
    <span v-if="isLoading" class="loading">Getting advice...</span>    
    <span v-else>"{{ adviceText }}"</span>
  </p>
</template>

<script setup>
const props = defineProps({
  adviceText: {
    type: String,
    required: true,
  },
  isLoading: {
    type: Boolean,
    required: true
  }
})

</script>

<style lang="scss" scoped>
$adviceText: hsl(193, 38%, 86%);

.advice__text {
  color: $adviceText;
  font-weight: 800;
  font-size: 1.5rem;

  .loading {
    font-size: 1.5rem;
  }
}

</style>