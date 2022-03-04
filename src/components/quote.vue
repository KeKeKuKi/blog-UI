<template>
  <div class="quote">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "quote"
}
</script>

<style scoped>
.quote {
  border-left: 5px solid #ff6d6d;
  background-color: rgba(255, 255, 255, 0.1);
  color: white;
  padding: 15px 20px;
  border-radius: 3px;
}
</style>
