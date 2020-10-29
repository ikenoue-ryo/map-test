<template>
  <div>
    <div class="map" ref="googleMap"/>
  </div>
</template>

<script src="https://maps.googleapis.com/maps/api/js?key=ここにAPIキー"></script>


<script>
export default {
  name: 'Map',
  data() {
    return {
      google: null,
      comments: null,
      mapConfig: {
        center: {
          lat: null,
          lng: null
        },
        zoom: 17
      },
      lat: '',
      lng: '',
    }
  },
  async mounted() {
    const URL = 'https://maps.googleapis.com/maps/api/geocode/json?new_forward_geocoder=true&address='
    const gapiKey = '&key=' + 'ここにAPIキー'
    const search_address = '福岡県福岡市中央区天神' 

    this.mapConfig.center.lat = 33.6000737 //①
    this.mapConfig.center.lng = 130.4279274 //②

    axios.get(URL + search_address + gapiKey)
    .then(response => { 
      console.log(response) //responseのデータを①と②に渡したい。 JSON：response.data.results[0].geometry.bounds.northeast.lng
      })
    .catch(response => console.log(response))

    this.google = await GoogleMapsApiLoader({
      apiKey: 'ここにAPIキー'
    });
    this.initializeMap();
  },
  methods: {
    initializeMap() {
      new this.google.maps.Map(this.$refs.googleMap, this.mapConfig);
    }
  }
}
</script>

<style scoped>

</style>