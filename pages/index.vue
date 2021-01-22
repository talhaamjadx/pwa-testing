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
      const device = await navigator.bluetooth.requestDevice({ acceptAllDevices: true }).then(res => {
        console.log(res)
        this.response = res
      })
      .catch(err => {
        console.log(err)
        this.response = err
      })
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
