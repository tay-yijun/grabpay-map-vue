<template>
  <div class="summary-card">
    <div
      class="w-100 p-1 border border-default text-dark"
    >Summary: {{ Number(merchantSummary).toLocaleString() }}</div>
  </div>
</template>

<script>
export default {
  name: "MerchantListSummaryCard",
  props: {
    merchantSummary: {
      type: Number, 
      required: true
    }
  }
};
</script>

<style scoped>
.summary-card {
  text-align: center;
}
</style>