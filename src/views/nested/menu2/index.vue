<template>
  <div>
    <h4 class="spectrum-view-title">Spectrum Usage</h4>
    <div
      v-for="item in Spectrum"
      v-bind:key="item"
    > 
      <div class="spectrum-elements">
        <div class="spectrum-range-text" :class="{ spectrum_available: !item.isUsed, spectrum_granted: item.usePriority == 1, spectrum_gaa: item.usePriority == 2, spectrum_pal: item.usePriority == 3, spectrum_incumbent: item.usePriority == 4 }">{{ item.name }}</div>
        <div v-if="!item.isUsed" class="spectrum-details-text">Available</div>
        <div v-else class="spectrum-details-text">Occupied - {{item.fccId}} - {{item.stateText}}</div>
      </div>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client';
export default {
  name: "Dashboard",
  data() {
    return {
      socket: io(),
      Spectrum: [
          {
            name: "3.550 - 3.560 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "CBSD-CCI-A",
          },
          {
            name: "3.560 - 3.570 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.570 - 3.580 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "CBSD-CCI-B",
          },
          {
            name: "3.580 - 3.590 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.590 - 3.600 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.600 - 3.610 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.610 - 3.620 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "CBSD-CCI-PAL-B",
          },
          {
            name: "3.620 - 3.630 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.630 - 3.640 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "",
          },
          {
            name: "3.640 - 3.650 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.650 - 3.660 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.660 - 3.670 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.670 - 3.680 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.680 - 3.690 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
          {
            name: "3.690 - 3.700 GHz",
            isUsed: false,
            usePriority: 0,
            fccId: "",
            stateText: '',
            cbsdId: "A",
          },
        ],
    };
  },
  sockets: {
        spectrumUpdate: function (data) {
          console.log("Spectrum update received")
          for (var i = 0; i < data.length; i+=1){
            console.log(data[i].fccId);
            this.Spectrum[data[i].subband - 1].fccId = data[i].fccId;
            this.Spectrum[data[i].subband - 1].cbsdId = data[i].cbsdId;
            this.Spectrum[data[i].subband - 1].stateText = data[i].stateText;

            if (data[i].state < 2){
              this.Spectrum[data[i].subband - 1].usePriority = 0;
              this.Spectrum[data[i].subband - 1].isUsed = false;
            }
            else if (data[i].state == 2){
              this.Spectrum[data[i].subband - 1].usePriority = 1;
              this.Spectrum[data[i].subband - 1].isUsed = true;
            }
            else{
              if (data.accessPriority == "PAL"){
                this.Spectrum[data[i].subband - 1].usePriority = 3;
                this.Spectrum[data[i].subband - 1].isUsed = true;
              }
              else{
                this.Spectrum[data[i].subband - 1].usePriority = 2;
                this.Spectrum[data[i].subband - 1].isUsed = true;
              }
            }
          }

        }
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

.spectrum-elements {
  padding: 8px;
  border-width: thin;
  border-color: #e9ecef;
  border-style: solid;
  background: #ffffff;
}
.spectrum_available {
  border-color: #289847;
  background: #b8efb8;
}

.spectrum_gaa {
  border-color: #bdba31;
  background: #eeefb8;
}

.spectrum_pal {
  border-color: #bd8d31;
  background: #efe3b8;
}

.spectrum_incumbent {
  border-color: #bd5231;
  background: #efb8b8;
}

.spectrum_granted {
  border-color: #315dbd;
  background: #b8cbef;
}

.spectrum-range-text {
  font-size: 12px;
  color: #212529;
  display: inline-block;
  border-style: solid;
  border-radius: 10px;
  padding: 5px;
  margin-left: 35px;
  padding-left: 30px;
  padding-right: 30px;
}

.spectrum-details-text {
  font-size: 15px;
  display: inline-block;
  margin-left: 20px;
}

.spectrum-view-title {
  padding-top: 18px;
  padding-left: 16px;
  padding-bottom: 8px;
}


.card-text-size {
  font-size: 12px !important;
  margin-left: 8px;
  margin-bottom: 8px;
}
</style>