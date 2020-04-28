<template>
  <div>
    <b>{{ merchant.properties.name }}</b>
    <p>{{ merchant.properties.address }}</p>
  </div>
</template>

<script>
export default {
  name: "MerchantListItem",
  props: {
    merchant: Object
  }
};
</script>

<style scoped>
</style>