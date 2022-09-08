
<template>
  <div>
    <div class="location">
      <h1>Ip Address Tracker</h1>
      <form action="">
        <input type="text" v-model="input">
        <button @click.prevent="changeIP">></button>
      </form>
    </div>
    <div class="container">
      <div class="sub-container">
        <div>
          <p>Ip Address</p>
          <h2>{{ip}}</h2>
        </div>
        <div>
          <p>location</p>
          <h2>{{location?.location?.region + ", " + location?.location?.city}}</h2>
        </div>
        <div>
          <p>TimeZone</p>
          <h2>{{location?.location?.timezone}}</h2>
        </div>
        <div>
          <p>isp</p>
          <h2>{{location?.isp}}</h2>
        </div>
      </div>
    </div>
    <iframe width="100%" height="500" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"
      :src="`https://maps.google.com/maps?q=${location?.location?.lat},${location?.location?.lng}&hl=en&z=14&amp;output=embed`">
    </iframe>
  </div>
</template>
    
<script>
import { onMounted, ref } from 'vue';
import axios from 'axios'
export default {
  setup() {
    const input = ref()
    const ip = ref()
    const location = ref([])
    onMounted(async () => {
      const res = await axios.get('https://api.ipify.org/?format=json')
      ip.value = res.data.ip
      const locationRes = await axios.get('https://geo.ipify.org/api/v2/country,city,vpn?apiKey=at_ncF5xe0nsomy384wA5xJnb6LzWLxL&ipAddress=' + ip.value)
      location.value = locationRes.data

    })
    const changeIP = async () => {
      ip.value = input.value
      const locationRes = await axios.get('https://geo.ipify.org/api/v2/country,city,vpn?apiKey=at_ncF5xe0nsomy384wA5xJnb6LzWLxL&ipAddress=' + ip.value)
      location.value = locationRes.data
    }
    return {
      ip,
      location,
      changeIP,
      input
    }
  }
}
</script>
    