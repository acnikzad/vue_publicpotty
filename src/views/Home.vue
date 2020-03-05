<template>
  <div class="home">
    <div id="map">
      <gmap-map
        :center="center"
        :zoom="zoom"
        map-type-id="roadmap"
        style="width: 100%; height: 600px">
        <gmap-marker
          :key="index"
          v-for="(m, index) in markers"
          :clickable="true"
          :draggable="false"
          :name="m.name"
          :position="m.position"
          :icon="markerOptions"
          @click="center=m.position"
        />
      </gmap-map>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
const startMarker = ('http://www.pngall.com/wp-content/uploads/2017/05/Map-Marker-Free-PNG-Image.png');

export default {
  data: function() {
    return {
      center: {lat:38.58166, lng:-121.49445},
      zoom: 13,
      markers:[],
      reviews:[],
      bathrooms: [],
      markerOptions: {
        url: startMarker,
        size: {width: 50, height: 50, f: 'px', b: 'px',},
        scaledSize: {width: 30, height: 30, f: 'px', b: 'px',}
      },
    };
  },
  name: 'Home',
  components: {

  },
  created: function() {
    axios.get(`/api/reviews`).then(response => {
      this.reviews = response.data;
      console.log(response);
    });

    axios.get(`/api/bathrooms`).then(response => {
      this.bathrooms = response.data;
      console.log(response);
      console.log(response["data"][0]);
      this.markers.push({
        park: response["data"][0]["park"],
        position: {lat:parseFloat(response["data"][0]["latitude"]), lng:parseFloat(response["data"][0]["longitude"])}});
      console.log(this.markers);
    });


    
  },
};

</script>
