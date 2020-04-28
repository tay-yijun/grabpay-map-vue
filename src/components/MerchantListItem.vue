<template>
  <div>
    <b>{{ merchant.properties.name }}</b>
    <b-button
      size="sm"
      class="mb-2"
      variant="outline"
      v-b-tooltip.hover
      title="Zoom!"
      @click="updateSelectedItem(merchant)"
    >
      <b-icon icon="geo-alt"></b-icon>
    </b-button>
    <p>{{ merchant.properties.address }}</p>
  </div>
</template>

<script>
export default {
  name: "MerchantListItem",
  props: {
    merchant: Object
  },
  methods: {
    updateSelectedItem(e) {
      this.$emit("update-selected-item", e);
    }
  }
};
</script>

<style scoped>
</style>