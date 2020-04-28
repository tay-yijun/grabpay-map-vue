<template>
  <div>
    <TheNavBar
      @update-sort="updateSort"
      @update-search-filter="updateSearchFilter"
      @update-filter-category="updateFilterCategory"
    />
    <b-container class="h-100" fluid>
      <b-row>
        <!-- Merchant List and Summary Card -->
        <b-col sm="4" order="1" class="h-100">
          <MerchantListSummaryCard :merchantSummary="merchantsSummary" />
          <MerchantList :merchants="orderedMerchants" />
        </b-col>
        <!-- MapItem -->
        <b-col sm="8" order="2" class="h-100">
          <MapItem :merchants="filteredMerchants" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
// import axios from "axios";
import _ from "lodash";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
import TheNavBar from "./components/TheNavBar";
import MapItem from "./components/MapItem";
import MerchantList from "./components/MerchantList";
import MerchantListSummaryCard from "./components/MerchantListSummaryCard";
import data from "./data/merchants.json";

export default {
  components: {
    TheNavBar,
    MapItem,
    MerchantList, 
    MerchantListSummaryCard
  },
  data() {
    return {
      // merchants: data[1],
      merchants: data[1].slice(0, 200),
      sortDirection: "asc",
      filterCategory: "",
      searchVal: "",
      selectedItem: {}
    };
  },
  computed: {
    // Add category property to data
    merchantsModified() {
      return this.merchants.map(merchant => ({
        ...merchant,
        category: this.setCategory(
          merchant.type_fnb,
          merchant.type_retail,
          merchant.type_service
        )
      }));
    },
    // Create geojson object
    merchantsGeojson() {
      let geojsonObj = {
        type: "FeatureCollection",
        features: []
      };
      this.merchantsModified.forEach(merchant => {
        if (
          // Check for valid latlng
          merchant.lat !== undefined ||
          merchant.lng !== undefined
        ) {
          let feature = {
            type: "Feature",
            properties: {
              name: merchant.mex_trading_name,
              category: merchant.category, 
              address: merchant.address
            },
            geometry: {
              type: "Point",
              coordinates: [merchant.lng, merchant.lat]
            }
          };
          geojsonObj.features.push(feature);
        }
      });
      return geojsonObj;
    },
    // Filter by searchVal
    filteredMerchantsBySearch() {
      let features = [];
      features = this.merchantsGeojson.features.filter(merchant => {
        return (
          // Match Merchant name
          merchant.properties.name
            .toLowerCase()
            .includes(this.searchVal.toLowerCase()) ||
          // Or match Merchant address
          merchant.properties.address.toLowerCase().includes(this.searchVal.toLowerCase())
        );
      });
      // features = this.merchantsGeojson.features.filter(merchant => {
      //   let value = this.searchVal.toLowerCase();
      //   let name = merchant.properties.name.toLowerCase();
      //   return name.indexOf(value) > -1;
      // });
      return features; 
    },
    // Filter by filterCategory
    filteredMerchantsByDropdown() {
      let features = [];
      if (this.filterCategory == "") {
        return this.filteredMerchantsBySearch;
      } else {
        features = this.filteredMerchantsBySearch.filter(
          merchant => merchant.properties.category == this.filterCategory
        );
        return features; 
      }
    }, 
    // Filtered combined
    filteredMerchants() {
      let geojsonObj = {
        type: "FeatureCollection",
        features: []
      };
      geojsonObj.features = this.filteredMerchantsByDropdown; 
      return geojsonObj; 
    }, 
    // Use orderBy in Lodash to re-order data
    orderedMerchants() {
      let geojsonObj = {
        type: "FeatureCollection",
        features: []
      };
      geojsonObj.features = _.orderBy(
        this.filteredMerchants.features,
        "properties.name", 
        this.sortDirection
      );
      return geojsonObj; 
    },
    merchantsSummary() {
      let merchantsCount = this.filteredMerchants.features.length; 
      return merchantsCount; 
    }
  },
  methods: {
    updateSearchFilter(e) {
      this.searchVal = e;
    },
    updateSort(e) {
      this.sortDirection = e;
    },
    updateFilterCategory(e) {
      this.filterCategory = e;
    },
    // Returns category string
    setCategory(type_fnb, type_retail, type_service) {
      if (type_fnb == "1") {
        return "fnb";
      } else if (type_retail == "1") {
        return "retail";
      } else if (type_service == "1") {
        return "service";
      } else {
        return "others";
      }
    }
  }
};
</script>

<style>
@import "https://api.tiles.mapbox.com/mapbox-gl-js/v1.8.1/mapbox-gl.css";
</style>
