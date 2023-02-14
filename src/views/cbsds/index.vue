<template>
  <div>
    <h4 class="spectrum-view-title">CBSDs</h4>
    <div
      v-for="item in Spectrum"
      v-bind:key="item"
    > 
      <div class="spectrum-elements">
        <!-- <div class="spectrum-range-text" :class="{ spectrum_available: !item.isUsed, spectrum_granted: item.usePriority == 1, spectrum_gaa: item.usePriority == 2, spectrum_pal: item.usePriority == 3, spectrum_incumbent: item.usePriority == 4 }">{{ item.name }}</div> -->
        <div class="spectrum-details-text">{{item.fccId}}</div>
        <div class="cbsd-status-text" :class="{ cbsd_reg: item.state == 1, cbsd_grant: item.state == 2, cbsd_trans: item.state == 3 }">{{ item.stateText }}</div>
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
          
        ],
    };
  },
  created(){
    this.$socket.emit('getCbsdList', '')
  },
  sockets: {
        connect: function () {
          console.log('socket connected')
          this.$socket.emit('getCbsdList', '')
        },
        cbsdUpdate: function (data) {
          console.log("Cbsd update received")
          this.Spectrum = data;
        },
        spectrumUpdate: function (data) {
          console.log("Spectrum update received")
          this.Spectrum = data;
          // for (var i = 0; i < data.length; i+=1){
          //   console.log(data[i].fccId);
          //   this.Spectrum[data[i].subband - 1].fccId = data[i].fccId;
          //   this.Spectrum[data[i].subband - 1].cbsdId = data[i].cbsdId;
          //   this.Spectrum[data[i].subband - 1].stateText = data[i].stateText;

          //   if (data[i].state < 2){
          //     this.Spectrum[data[i].subband - 1].usePriority = 0;
          //     this.Spectrum[data[i].subband - 1].isUsed = false;
          //   }
          //   else if (data[i].state == 2){
          //     this.Spectrum[data[i].subband - 1].usePriority = 1;
          //     this.Spectrum[data[i].subband - 1].isUsed = true;
          //   }
          //   else{
          //     if (data.accessPriority == "PAL"){
          //       this.Spectrum[data[i].subband - 1].usePriority = 3;
          //       this.Spectrum[data[i].subband - 1].isUsed = true;
          //     }
          //     else{
          //       this.Spectrum[data[i].subband - 1].usePriority = 2;
          //       this.Spectrum[data[i].subband - 1].isUsed = true;
          //     }
          //   }
          // }

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