<template>
  <div class="dashboard-container">
    <div>
    <GoogleMap/>
    </div>
    <div>
      <b-button hidden v-b-toggle.sidebar-no-header>Toggle Sidebar</b-button>
      <b-sidebar
        id="sidebar-no-header"
        aria-labelledby="sidebar-no-header-title"
        no-header
        right
        shadow
      >
        <template #default="{ hide }">
          <div class="p-3">
            <b-card v-if="Requests.registrationRequest.length">
              <b-list-group flush>
                <b-list-group-item></b-list-group-item>
                <b-list-group-item></b-list-group-item>
              </b-list-group>
              <b-card-text class="card-text-size"
                ><h6>Device info</h6></b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>CBSD category:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}
              </b-card-text>
              <b-card-text class="card-text-size"
                ><b>Radio type:</b> empty</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>User type:</b>
                {{ Requests.registrationRequest[0].userId }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>FCC ID:</b>
                {{ Requests.registrationRequest[0].fccId }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Serial number:</b>
                {{
                  Requests.registrationRequest[0].cbsdSerialNumber
                }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Device type:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Max EIRP:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-list-group flush>
                <b-list-group-item></b-list-group-item>
                <b-list-group-item></b-list-group-item>
              </b-list-group>
              <b-card-text class="card-text-size"><h6>Antenna</h6></b-card-text>
              <b-card-text class="card-text-size"
                ><b>Antenna model</b>
                {{ Requests.registrationRequest[0].userId }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Max gain:</b>
                {{ Requests.registrationRequest[0].fccId }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Beam width:</b>
                {{
                  Requests.registrationRequest[0].cbsdSerialNumber
                }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Height:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Height type:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Azimuth:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Mech. downtilt:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Horiz. accuracy:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Vert. accuracy:</b>
                {{ Requests.registrationRequest[0].cbsdCategory }}</b-card-text
              >
              <b-list-group flush>
                <b-list-group-item></b-list-group-item>
                <b-list-group-item></b-list-group-item>
              </b-list-group>
              <b-button variant="primary" block @click="hide"
                >Close details</b-button
              >
            </b-card>
          </div>
        </template>
      </b-sidebar>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import GoogleMap from "../../components/GoogleMap.vue";
export default {
  name: "Dashboard",
  data() {
    return {
      done: true,
      markers: [
     {
      position:
        {
          lat: 38.88092897208466,
          lng: -77.11567323046333
        },
      icon: {
        url: "require (`@/assets/logos/Horizontal_VT_Full_Color_RGB.png`)",
        scaledSize: {width: 28, height: 28},
        labelOrigin: {x: 16, y: -10}
        },
      title: 'title',
      label: {
        text: 'label',
        color: "black",
        fontWeight: "bold",
        fontSize: "12px"
      }
    }
    ],
      Requests: {
        registrationRequest: [
          {
            fccId: "CCI-CBRS-001",
            userId: "virginia-tech",
            cbsdSerialNumber: "cbsdnotrealserial",
            cbsdCategory: "A",
            airInterface: {
              radioTechnology: "E_UTRA",
            },
            cbsdInfo: {
              vendor: "ETTUS",
              model: "x310",
              softwareVersion: "v0.1",
              hardwareVersion: "v0.1",
              firmwareVersion: "v0.1",
            },
            groupingParam: [
              {
                groupType: "PRINCIPAL_SUBORDINATE_SFG",
                groupId: "3560",
              },
              {
                groupType: "SPECTRUM_REUSE",
                groupId: "",
              },
            ],
          },
        ],
      },
    };
  },
  components: {
    GoogleMap,
  },
  
  created(){
    this.$root.$on('cbsd_clicked', (text) => {
      this.text = text;
      this.$root.$emit('bv::toggle::collapse', 'sidebar-no-header')
    });
    
  },
  sockets: {
        connect: function () {
            console.log('socket connected')
        },
        registrationResponse: function (data) {
            console.log('this method was fired by the socket server. eg: io.emit("customEmit", data)')
        }
  },
  computed: {
    ...mapGetters(["name"]),
  },
};
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}

.card-text-size {
  font-size: 12px !important;
  margin-left: 8px;
  margin-bottom: 8px;
}
</style>
