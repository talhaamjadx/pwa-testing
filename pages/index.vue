<template>
  <div class="container">
    <button @click="connectBluetooth()">Connect Bluetooth</button>
    <button @click="sensor()">Sensor</button>
    <button @click="contactPicker()">Contacts</button>
    <input type="file" accept="image/*" capture="camera" />
    {{ response }}
  </div>
</template>

<script>
export default {
  data(){
    return{
      response: ""
    }
  },
  mounted(){
    window.navigator.geolocation.getCurrentPosition(console.log)
    window.addEventListener('devicemotion', (event) => {
      console.log(event)
    })
  },
  methods:{
    async connectBluetooth(){
      const device = await navigator.bluetooth.requestDevice({ filters: [{ services: ['heart_rate'] }] });
      const server = await device.gatt.connect();

      // Get heart rate data
      const hr = await server.getPrimaryService('heart_rate');
      const hrMeasurement = await hr.getCharacteristic('heart_rate_measurement');

      // Listen to changes on device
      await hrMeasurement.startNotifications(); 

      hrMeasurement.addEventListener('characteristicvaluechanged', (e) => {
          console.log(parseHeartRate(e.target.value));
      });
    },
    async sensor(){
      if('IdleDetector' in window){
          const state = await IdleDetector.requestPermission()
          alert(state)
      }
    },
    async contactPicker(){
      const props = ['name', 'email', 'tel', 'address', 'icon'];
      const opts = {multiple: true};
      const supported = ('contacts' in navigator && 'ContactsManager' in window);
      alert(supported)
          if (supported) {
              const contacts = await navigator.contacts.select(props, opts);
              console.log(contacts)
          }
    }
  }
}
</script>
<style>
</style>
