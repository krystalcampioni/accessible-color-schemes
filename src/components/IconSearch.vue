<template>
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 50 50">
    <g fill="none" fill-rule="evenodd" :stroke="color">
      <circle cx="21" cy="20" r="16" stroke-linecap="round" stroke-width="2"/>
      <path stroke-width="4" d="M32 32l13 13"/>
    </g>
  </svg>
</template>

<script>
export default {
  props: {
    color: {
      type: String,
      default: "black"
    }
  }
};
</script>

<style scoped>
svg {
  width: 100%;
}
</style>
