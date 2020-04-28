<template>
  <div class="merchant-list">
    <b-list-group>
      <div v-for="merchant in merchants.features" :key="merchant.id">
        <b-list-group-item>
          <MerchantListItem :merchant="merchant" />
        </b-list-group-item>
      </div>
    </b-list-group>
  </div>
</template>

<script>
import MerchantListItem from "./MerchantListItem";

export default {
  name: "MerchantList",
  props: {
    merchants: {
      type: Object,
      required: true
    }
  },
  components: {
    MerchantListItem
  }
};
</script>

<style scoped>
.merchant-list {
  overflow-y: auto;
  height: 85vh;
}
</style>