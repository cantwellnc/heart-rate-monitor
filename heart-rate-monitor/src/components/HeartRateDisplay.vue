<template>
  <div class="hello">
    <div v-if = "isDeviceConnected">
      <h1>connected!</h1> 
      <h3>{{ heartRate }}</h3>
    </div>
    <button v-if = "!isDeviceConnected" @click = "requestBluetoothConnection">Search for Bluetooth Devices</button>
    <button v-else @click = "revokeBluetoothConnection">Disconnect</button>
  </div>
</template>

<script>
export default {
  name: 'HeartRateDisplay',

  data() {
    return { heartRate: 0, isDeviceConnected: false, device: {}, server: {}}; 
  }, 

  methods: {

    async requestBluetoothConnection() {
      try{
        const device = await navigator.bluetooth.requestDevice({
        filters: [{
          name: "Versa Lite",
        }], 
        optionalServices: ["heart_rate"],
        }); 
        this.device = device; 
        const server = await device.gatt.connect();
        if(server){
          this.isDeviceConnected = true;
          this.server = server;
          this.getHeartRate(server); 
        }

      }
      catch(error){
        console.warn(error);
      }
       
    }, 

    async getHeartRate(server){
      // Sooooo unfortunately fitbit doesn't allow data to be transmitted over
      // direct BLE connection, so we can't access this service w/o going through
      // their API which kinda defeats the purpose of this whole thing
      try{
        const hr = await server.getPrimaryService("heart_rate");
        const hrMeasurement = await hr.getCharateristic("heart_rate_measurement");
        this.heartRate = hrMeasurement.readValue().getUint8(0);
      }
      catch(error){
        console.warn(error);
      }
      
    }, 

    revokeBluetoothConnection(){
      this.device.gatt.disconnect();
      this.isDeviceConnected = false;
    }, 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  margin: 40px 0 0;
}

button {
  font-weight: 600;
  padding: 10px 10px;
  background-color: #385e3c;
  color: #fefff1;
  border-radius: 10px;
  border: none;
  box-shadow: 2px 2px 5px #111111;
  transition: background-color 500ms; 
}
button:hover {
  background-color: #486a47;
}
</style>
