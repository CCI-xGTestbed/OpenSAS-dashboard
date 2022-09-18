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
      :zoom='zoom'
      style='width: 100%;  height: 800px;'
    >
    <div :key="index"
        v-for="(item, index) in this.List"
        :aria-label="item.cbsdId"
        
        >
        <div :id="item.cbsdId">
      <GmapMarker
        :position="item.position"
        :label="item.fccId"
        :icon="require (`@/assets/logos/wifi-logo.png`)"
        v-on:click="cbsdClicked(item.cbsdId)"
      />
    </div>
    </div>
    </GmapMap>
  </div>
</template>

<script>
export default {
  props: [ 'List'  ],
  watch: {
    $props: {
      handler() {
        if (this.$props.List.length > 0){
          this.center = this.List[0].position;
          this.zoom = 19;
        }
      },
      deep: true,
      immediate: true,
    },
  },
  name: 'GoogleMap',
  data() {
    return {
      center: { lat: 39.29647910169052, lng: -98.10019483966342 },
      zoom: 5,
      currentPlace: null,
      markers: [{
      position:
        {
          lat: 38.88092897208466,
          lng: -77.11567323046333
        },
      icon: {
        url: require (`@/assets/logos/wifi-logo.png`),
        scaledSize: {width: 40, height: 40},
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
    cbsdClicked(event){
      this.$root.$emit('cbsd_clicked', event)
      console.log(this.List)
      
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