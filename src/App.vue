<template>
  <div>
    <input type="text" v-model="address">
    <button type="button" @click="mapSearch">検索</button>
    {{ ido }}
    <div id="map"></div>
    
  </div>
</template>


<script>
export default {
  name: 'Map',
  metaInfo: {
    script: [
      { src: `https://maps.googleapis.com/maps/api/js?key=${process.env.VUE_APP_GOOGLE_MAP_API}&callback=initMap`, async: true, defer: true }
    ],
  },
  data() {
    return {
      map: {},
      marker: null,
      geocoder: {},
      address: '福岡県福岡市博多区堅粕',
      ido: '',
      keido: ''
    }
  },
  mounted() {
    window.initMap = () => {
      this.map = new window.google.maps.Map(document.getElementById('map'),
      {
        center: {
          lat: this.ido,
          lng: this.keido
        },
        zoom: 19
      }
      );
      this.geocoder = new window.google.maps.Geocoder();
    }
  },
  methods: {
    mapSearch() {
      this.geocoder.geocode({
        'address': this.address,
        newForwardGeocoder:true
      }, (results, status) => {
        if (status === window.google.maps.GeocoderStatus.OK) {
          this.map.setCenter(results[0].geometry.location);
          // 緯度経度の取得
          results[0].geometry.location.lat();
          results[0].geometry.location.lng();
          this.marker = new window.google.maps.Marker({
            map: this.map,
            position: results[0].geometry.location
          });
          console.log(results)
          this.ido = results[0].geometry.location.lat()
          this.keido = results[0].geometry.location.lng()
        }
      });
    }
  }
}
</script>

<style>
#map {
  width: 600px;
  height: 600px;
}
</style>

