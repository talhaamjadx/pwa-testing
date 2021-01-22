<template>
  <div class="container">
    <button @click="connectBluetooth()">Connect Bluetooth</button>
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
    }
  }
}
</script>
<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
