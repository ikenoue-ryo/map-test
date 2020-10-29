<template>
  <div>
    <input type="text" v-model="address">
    <button type="button" @click="mapSearch">検索</button>
    <div id="map"></div>
  </div>
</template>

<script src="https://maps.googleapis.com/maps/api/js?key=ここにAPIキー"></script>


<script>
export default {
  name: 'Map',
  data() {
    return {
      map: {},
      marker: null,
      geocoder: {},
      address: '福岡県福岡市博多区'
    }
  },
  mounted() {
    this.map = new google.maps.Map(document.getElementById('map'));
    this.geocoder = new google.maps.Geocoder();
  },
  methods: {
    mapSearch() {
      this.geocoder.geocode({
        'address': this.address,
        newForwardGeocoder:true
      }, (results, status) => {
        if (status === google.maps.GeocoderStatus.OK) {
          this.map.setCenter(results[0].geometry.location);
          // 緯度経度の取得
          // results[0].geometry.location.lat();
          // results[0].geometry.location.lng();
          this.marker = new google.maps.Marker({
            map: this.map,
            position: results[0].geometry.location
          });
        }
      });
    }
  }
}
</script>

<style scoped>

</style>