<template>
  <div class="container">
    <button @click="connectBluetooth()">Connect Bluetooth</button>
    <button @click="sensor()">Sensor</button>
    <button @click="contactPicker()">Contacts</button>
    <input type="file" accept="image/*" capture="camera" />
    {{ response }}
    <button @click="fileSystemAPI()">Select a file</button>
    <textarea v-model="contents" name="" id="" cols="30" rows="10"></textarea>
    <button @click="saveAs()">Save As</button>
  </div>
</template>

<script>
export default {
  data(){
    return{
      response: "",
      contents: "",
    }
  },
  mounted(){
    window.navigator.geolocation.getCurrentPosition(console.log)
    window.addEventListener('devicemotion', (event) => {
      //console.log(event)
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
      console.log(window)
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
    },

    async createNewFile(){
      const options = {
        types: [
          {
            description: 'Text Files',
            accept: {
              'text/plain': ['.txt'],
            },
          },
        ],
      }
      const handle = await window.showSaveFilePicker(options);
         return handle;
    },

    async saveAs(){
      let newFileHandle = await this.createNewFile()
      let writable = await newFileHandle.createWritable()
      await writable.write(this.contents);
      await writable.close();
    },

    async fileSystemAPI(){
      let [fileHandle] = await window.showOpenFilePicker()
      console.log({fileHandle})
      let file = await fileHandle.getFile()
      this.contents = await file.text()
      console.log(this.contents)
    }
  }
}
</script>
<style>
</style>
