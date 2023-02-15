<template>
  <div>
    <!-- <h4 class="spectrum-view-title">Environment Sensing</h4> -->
    <div>
    <h4 class="spectrum-view-title">Spectrum Sensing</h4>
    <h8 class="spectrum-view-subtitle">  Presents power level in each 10 MHz channel from RF Sensors</h8>
    </div>
    <div>
    <div class="sensor-view">
      <div class="power-bar-holder">
        <div v-for="channel in powers.channels" :key="channel">
          <div class="power-bar" :style="{ bottom:  (0 + 3 * channel.power) + 'px', height:  + (400 + 3 * channel.power) + 'px'}"><div class="power-value">{{ channel.power + "dBm" }}</div></div>
          
        </div>
        <div class="power-bar-invisible" :style="{ bottom:  0 + 'px', height:  + (400 ) + 'px'}"></div>
      </div>
      <div class="text-bar-holder">
        <div v-for="channel,index in powers.channels" :key="channel">
          <div class="channel-text">Ch{{index + 1}}</div>
        </div>
      </div>
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
        powers: {
          channels:[
            {name: "Channel1", power: -100},{name: "Channel2", power: -100},{name: "Channel3", power: -100},{name: "Channel4", power: -100},{name: "Channel5", power: -100},{name: "Channel6", power: -100},{name: "Channel7", power: -100},{name: "Channel8", power: -100},{name: "Channel9", power: -100},{name: "Channel10", power: -100},{name: "Channel11", power: -100},{name: "Channel12", power: -100},{name: "Channel14", power: -100},{name: "Channel15", power: -100}
          ]
        }
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
        },
        sensorUpdate: function (data) {
          console.log("Sensing update received");
          this.powers = data;
        },
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

.sensor-view{
  display: flex;
  flex-direction: column;
  height: 100vh;
}
.power-bar-holder {
  position: relative;
  bottom: 0;
  margin-top: 35vh;
  margin-left: 20px;
  display: flex;
}
.power-bar {
    width: 30px;
    background-color: rgb(123, 205, 0);
    float: left;
    margin: 20px;
    display: list-item;
    list-style-type: none;
    position: relative;
}

.power-value {
  position: relative;
  bottom: 24px;
  font-size: 12px;
  right: 10px;
  font-weight: bold;
}

.power-bar-invisible {
    width: 30px;
    background-color: rgb(255, 255, 255);
    float: left;
    margin: 15px;
    display: list-item;
    list-style-type: none;
    position: relative;
}

.channel-text {
    width: 30px;
    float: left;
    margin: 20px;
    display: list-item;
    list-style-type: none;
    position: relative;
    font-weight: bold;
}

.text-bar-holder {
  position: relative;
  margin-left: 18px;
  bottom: 10px;
  display: flex;
  flex-grow: 1;
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

.spectrum-view-subtitle {
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