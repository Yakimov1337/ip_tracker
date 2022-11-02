<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div
      class="flex z-20 justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32"
    >
      <!-- Search Input    -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Address Tracker</h1>
        <div class="flex">
          <input
            v-model="queryIp"
            class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none"
            type="text"
            placeholder="Search for any IP address or leave empty to get your ip info"
          />
          <i
            @click="getIpInfo"
            class="cursor-pointer bg-black text-white px-4 border border-gray-900 rounded-tr-md rounded-br-md flex items-center fas fa-chevron-right"
          ></i>
        </div>
      </div>
      <!-- IP Info -->
      <IPInfo v-if="ipInfo" v-bind:ipInfo="ipInfo" />
    </div>

    <!-- Map -->
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
import IPInfo from "@/components/IPInfo.vue";
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios";

export default {
  name: "Home",
  components: { IPInfo },
  setup() {
    let map;

    const queryIp = ref("");
    const ipInfo = ref(null);

    onMounted(() => {
      map = leaflet.map("map").setView([42.5145, -83.0147], 5.8);
      leaflet
        .tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
          maxZoom: 19,
          attribution:
            '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
        })
        .addTo(map);
    });

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://api.ipgeolocation.io/ipgeo?apiKey=${process.env.VUE_APP_GEO_TOKEN}&ip=${queryIp.value}`
        );
        const result = data.data;
        ipInfo.value = {
          address: result.ip,
          state: result.continent_name,
          timezone: result.time_zone.current_time,
          isp: result.isp,
          lat: result.latitude,
          lng: result.longitude,
        };
        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(map);
        map.setView([ipInfo.value.lat, ipInfo.value.lng], 13);
    
      } catch (err) {
        alert(err.message);
      }
    };

    return { queryIp, ipInfo, getIpInfo };
  },
};
</script>
