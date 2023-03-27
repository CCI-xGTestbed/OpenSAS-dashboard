<template>
  <div>   
    <GmapMap :center="center" :zoom="zoom" style="width: 100%;  height: 800px;">
      <div :key="index" v-for="(item, index) in this.List" :aria-label="item.cbsdId">
        <div :id="item.cbsdId">
          <GmapMarker
            :position="item.position"
            :icon="require('@/assets/logos/antenna-6.png')"
            :label="item.fccId"
            v-on:click="cbsdClicked(item.cbsdId)"
          />
          <GmapCircle
            v-if="item.state === 3"
            :center="item.position"
            :radius="item.distance"
            :options="{ fillColor: '#FF0000', fillOpacity: 0.2, strokeWeight: 0 }"
          />

        </div>
      </div>
      <div
      v-for="(item, index) in this.Dpas"
      :key="index"
      :aria-label="item.id"
    >
      <div :id="item.id">
        <GmapMarker
          v-if="item.active"
          :position="{ lat: item.latitude, lng: item.longitude }"
          :icon="require('@/assets/logos/antenna-6.png')"
          :label="item.id"
        />
        <GmapCircle
          v-if="item.active"
          :center="{ lat: item.latitude, lng: item.longitude }"
          :radius="item.radius"
          :options="{
            fillColor: '#87CEFA',
            fillOpacity: 0.4,
            strokeWeight: 0,
          }"
        />
      </div>
    </div>
    </GmapMap>
  </div>
</template>

<script>
export default {
  props: [ 'List', 'Dpas'  ],
  watch: {
    $props: {
      handler() {
        if (this.$props.List.length > 0){
          if(this.$route.query.lat){
            var location_rec = {lat: parseFloat(this.$route.query.lat), lng: parseFloat(this.$route.query.long)}
            console.log(location_rec)
            this.center = location_rec
            
          }
          else{
            this.center = this.List[0].position;
          }
          this.zoom = 18;
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
      zoom: 8,
      currentPlace: null,
      markers: [{
      position:
        {
          lat: 38.88092897208466,
          lng: -77.11567323046333
        },
      icon: {
        url: require (`@/assets/logos/antenna-6.png`),
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