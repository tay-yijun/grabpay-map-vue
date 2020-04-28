<template>
  <div>
    <b-navbar toggleable="sm" type="light" variant="white">
      <b-navbar-brand>
        <img src="../assets/grab_logo.png" alt="logo" class="logo" />
      </b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <!-- Input Form -->
          <b-form-input
            class="mr-sm-2"
            size="sm"
            placeholder="Search"
            v-model="searchVal"
            @input="updateSearchFilter(searchVal)"
          ></b-form-input>
          <!-- Select Form -->
          <b-form-select
            class="mr-sm-2"
            size="sm"
            v-model="filterCategory"
            :options="filterCategoryOptions"
            @change="updateFilterCategory(filterCategory)"
          ></b-form-select>
          <!-- Select Form -->
          <b-form-select
            class="mr-sm-2"
            size="sm"
            v-model="sortDirection"
            :options="sortDirOptions"
            @change="updateSort(sortDirection)"
          ></b-form-select>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: "TheNavBar",
  data() {
    return {
      searchVal: "",
      sortDirection: "asc",
      filterCategory: "",
      sortDirOptions: [
        { value: "asc", text: "Sort: Ascending" },
        { value: "desc", text: "Sort: Descending" }
      ],
      filterCategoryOptions: [
        { value: "", text: "Filter" },
        { value: "fnb", text: "F&B" }, 
        { value: "retail", text: "Retail" }, 
        { value: "service", text: "Services" }, 
        { value: "others", text: "Others" }, 
      ]
    };
  },
  methods: {
    // // Methods to send filter options to Parent
    updateSearchFilter(e) {
      this.$emit("update-search-filter", e);
    },
    updateSort(e) {
      this.$emit("update-sort", e);
    },
    updateFilterCategory(e) {
      this.$emit("update-filter-category", e);
    }
  }
};
</script>

<style scoped>
.logo {
  max-height: 32px;
}
</style>