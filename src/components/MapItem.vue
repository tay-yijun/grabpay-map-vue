<template>
  <div id="map" class="mapbox-map"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";

export default {
  name: "MapItem",
  props: {
    merchants: {
      type: Object
    }
  },
  mounted() {
    // Token should be stored somewhere safe
    mapboxgl.accessToken =
      "pk.eyJ1IjoieWlqdW4iLCJhIjoiY2lnZGgzOGZrMXlwdnYybTc3Y2d5bDI1cyJ9.GGEWspgMgBkyZ7BITP7-5Q";

    // Create Map object
    this.map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/mapbox/light-v10",
      center: [103.8256, 1.35],
      zoom: 10.2
    })
      // Add navigation controls to the map
      .addControl(new mapboxgl.NavigationControl());

    // Create empty objects
    this.popup = new mapboxgl.Popup({
      closeButton: false
    });
    this.marker = new mapboxgl.Marker();

    // Do something when Map loads
    this.map.on("load", () => {
      this.map
        .addSource("merchantsLayer", {
          type: "geojson",
          data: this.merchants
        })
        .addLayer({
          id: "merchantsLayer",
          type: "circle",
          source: "merchantsLayer",
          paint: {
            "circle-radius": 5,
            "circle-opacity": 0.5,
            "circle-color": "#51bbd6"
          }
        });

      // Add popup on contriesLayer mouseover
      this.map.on("mouseenter", "merchantsLayer", e => {
        this.map.getCanvas().style.cursor = "pointer";
        // Populate the popup
        var feature = e.features[0];
        this.popup
          .setLngLat(feature.geometry.coordinates)
          .setText(feature.properties.name)
          .addTo(this.map);
      });

      // Remove cursor and popup on contriesLayer mouseleave
      this.map.on("mouseout", "merchantsLayer", () => {
        this.map.getCanvas().style.cursor = "";
        this.popup.remove();
      });
    });
  },
  methods: {
  },
  watch: {
    // Update layer when props changes
    merchants: function() {
      this.map.getSource("merchantsLayer").setData(this.merchants); 
    }
  }, 
  // computed: {
  //   // Parse props to geojson object
  //   merchantsGeojson () {
  //     let geojsonLayer = {
  //       type: "FeatureCollection",
  //       features: []
  //     }; 
  //     this.merchants.forEach(merchant => {
  //       // if (
  //       //   // Check for valid latlng
  //       //   merchant.lat !== undefined ||
  //       //   merchant.lng !== undefined
  //       // ) {
  //         let feature = {
  //           type: "Feature",
  //           properties: {
  //             name: merchant.mex_trading_name,
  //             category: merchant.category
  //           },
  //           geometry: {
  //             type: "Point",
  //             coordinates: [merchant.lng, merchant.lat]
  //           }
  //         };
  //         geojsonLayer.features.push(feature);
  //       // }
  //     });
  //     return geojsonLayer;
  //   }
  // }
};
</script>

<style scoped>
.mapbox-map {
  height: 90vh;
}
</style>