<template>
  <div class="dashboard-container">
    <div>
    <GoogleMap :List="CbsdList" :Dpas="DpaList"/>
    </div>
    <div :key="index"
        v-for="(item, index) in CbsdList">
      <b-button hidden v-b-toggle="item.cbsdId">Toggle Sidebar</b-button>
      <b-sidebar
        :id="item.cbsdId"
        aria-labelledby="sidebar-no-header-title"
        no-header
        right
        shadow
      >
        <template #default="{ hide }">
          <div class="p-3">
            <b-card v-if="CbsdList.length">
              <b-card-text class="card-text-size">
                <b>CBSD Status:</b>
                <div class="cbsd-status-text" :class="{ cbsd_reg: item.state == 1, cbsd_grant: item.state == 2, cbsd_trans: item.state == 3 }">{{ item.stateText }}</div>
              </b-card-text>
              <b-card-text v-if="item.state > 1" class="card-text-size"
                ><b>Frequency:</b>
                {{ item.spectrum.low }} GHz - {{ item.spectrum.high}} GHz
              </b-card-text>
              <b-card-text v-if="item.state > 1" class="card-text-size"
                ><b>Power:</b>
                {{ item.spectrum.power }} dBm
              </b-card-text>
              <b-list-group flush>
                <b-list-group-item></b-list-group-item>
                <b-list-group-item></b-list-group-item>
              </b-list-group>
              <b-card-text class="card-text-size"
                ><b>CBSD category:</b>
                {{ item.cbsdCategory }}
              </b-card-text>
              <b-card-text class="card-text-size"
                ><b>User type:</b>
                {{ item.userId }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>FCC ID:</b>
                {{ item.fccId }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b> Indoor device:</b> {{ item.installationParam.indoorDeployment }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Serial number:</b>
                {{
                  item.cbsdSerialNumber
                }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Device type:</b>
                {{ item.cbsdCategory }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Max EIRP:</b>
                {{ item.installationParam.eirpCapability }}</b-card-text
              >
              <b-list-group flush>
                <b-list-group-item></b-list-group-item>
                <b-list-group-item></b-list-group-item>
              </b-list-group>
              <b-card-text class="card-text-size"><h6>Antenna</h6></b-card-text>
              <!-- <b-card-text class="card-text-size"
                ><b>Antenna model</b>
                {{ item.userId }}</b-card-text
              > -->
              <b-card-text class="card-text-size"
                ><b>Gain:</b>
                {{ item.installationParam.antennaGain }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Beam width:</b>
                {{
                  item.installationParam.antennaBeamwidth
                }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Height:</b>
                {{ item.installationParam.height }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Height type:</b>
                {{ item.installationParam.heightType }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Azimuth:</b>
                {{ item.installationParam.antennaAzimuth }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Mech. downtilt:</b>
                {{ item.installationParam.antennaDowntilt }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Horiz. accuracy:</b>
                {{ item.installationParam.horizontalAccuracy }}</b-card-text
              >
              <b-card-text class="card-text-size"
                ><b>Vert. accuracy:</b>
                {{ item.installationParam.verticalAccuracy }}</b-card-text
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
import io from 'socket.io-client';
export default {
  name: "Dashboard",
  data() {
    return {
      socket: io(),
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
      CbsdList: 
        [ 
          // {
          //   fccId: "CCI-CBRS-001",
          //   userId: "virginia-tech",
          //   cbsdSerialNumber: "cbsdnotrealserial",
          //   cbsdCategory: "A",
          //   airInterface: {
          //     radioTechnology: "E_UTRA",
          //   },
          //   cbsdInfo: {
          //     vendor: "ETTUS",
          //     model: "x310",
          //     softwareVersion: "v0.1",
          //     hardwareVersion: "v0.1",
          //     firmwareVersion: "v0.1",
          //   },
          //   groupingParam: [
          //     {
          //       groupType: "PRINCIPAL_SUBORDINATE_SFG",
          //       groupId: "3560",
          //     },
          //     {
          //       groupType: "SPECTRUM_REUSE",
          //       groupId: "",
          //     },
          //   ],
          //   'installationParam': {
          //       'latitude': 38.880944239539396,
          //       'longitude': -77.11576958075611,
          //       'height': 1.0,
          //       'heightType': "AGL",
          //       'indoorDeployment': true,
          //       'antennaGain': 5
          //   },
          //   'position':{
          //     lat: 38.88092897208466,
          //     lng: -77.11567323046333
          //   }, 
          // },
        ],
      SpectrumList: [],
      DpaList: [],
    };  
  },
  components: {
    GoogleMap,
  },
  
  created(){
    this.$socket.emit('getCbsdList', '')
    this.$socket.emit('getDpaList', '')
    this.$root.$on('cbsd_clicked', (id) => {
      console.log(id)
      this.$root.$emit('bv::toggle::collapse', id)
    });
    this.socket.on('registrationResponse', (data) => {
      console.log('this method was fired by the socket server.')
    });
    this.socket.on('test', (data) => {
      console.log('this method was fired by the socket server.')
    });
    // this.socket.listener.subscribe("registrationResponse", (data) => {
    //     console.log("users", data);
    // });
  },
  sockets: {
        connect: function () {
          console.log('socket connected')
          this.$socket.emit('getCbsdList', '')
          this.$socket.emit('getDpaList', '')
        },
        disconnect: function() {
          console.log('socket disconnected')
        },
        registrationResponse: function (data) {
          console.log('this method was fired by the socket server.')
        },
        cbsdUpdate: function (data) {
          console.log("Cbsd update received")
          this.CbsdList = data;
        },
        spectrumUpdate: function (data) {
          console.log("Spectrum update received")
          this.SpectrumList = data;
        },
        dpaUpdate: function (data) {
          console.log("Dpa update received")
          this.DpaList = data;
        },
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

.cbsd_reg {
  border-color: #2568c6;
  background: #b8c8ef;
}

.cbsd_grant {
  border-color: #bd8d31;
  background: #efe3b8;
}

.cbsd_trans {
  border-color: #31bd4b;
  background: #b8efcb;
}

.cbsd-status-text {
  font-size: 12px;
  color: #212529;
  display: inline-block;
  border-style: solid;
  border-radius: 10px;
  padding: 5px;
  margin-left: 15px;
  padding-left: 30px;
  padding-right: 30px;
}

</style>
