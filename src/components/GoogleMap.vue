<template>
  <div>
    <div>
      <GmapAutocomplete
        @place_changed='setPlace'
      />
      <button
        @click='addMarker'
      >
        Add
      </button>
    </div>
    <br>
    <GmapMap
      :center='center'
      :zoom='12'
      style='width: 100%;  height: 800px;'
    >
      <GmapMarker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :icon="require (`@/assets/logos/wifi-logo.png`)"
        @click="center=m.position"
        v-on:click="cbsdClicked"
      />
    </GmapMap>
  </div>
</template>

<script>
export default {
  name: 'GoogleMap',
  data() {
    return {
      center: { lat: 45.508, lng: -73.587 },
      currentPlace: null,
      markers: [{
      position:
        {
          lat: 38.88092897208466,
          lng: -77.11567323046333
        },
      icon: {
        url: "require (`@/assets/logos/Horizontal_VT_Full_Color_RGB.png`)",
        scaledSize: {width: 2, height: 2},
        labelOrigin: {x: 16, y: -10}
        },
      title: 'title',
      label: {
        text: 'label',
        color: "black",
        fontWeight: "bold",
        fontSize: "12px"
      }
    }],
      places: [],
    }
  },
  mounted() {
    this.geolocate();
  },
  methods: {
    cbsdClicked(){
      this.$root.$emit('cbsd_clicked', 'do deed');
    },
    setPlace(place) {
      this.currentPlace = place;
    },
    addMarker() {
      if (this.currentPlace) {
        const marker = {
          lat: this.currentPlace.geometry.location.lat(),
          lng: this.currentPlace.geometry.location.lng(),
        };
        this.markers.push({ position: marker });
        this.places.push(this.currentPlace);
        this.center = marker;
        this.currentPlace = null;
      }
    },
    geolocate: function() {
      navigator.geolocation.getCurrentPosition(position => {
        this.center = {
          lat: position.coords.latitude,
          lng: position.coords.longitude,
        };
      });
    },
  },
};
</script>