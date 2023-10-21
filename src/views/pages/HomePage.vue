<template>
  <input />
  <button @click="getLocation()"></button>
  {{ location }}
  <button @click="searchFood">Tìm đồ ăn</button>
</template>
<script setup lang="ts">
import { ref, onMounted } from 'vue'
import {search} from '@/api/baseAPI.js'

onMounted(() => {
    handlePermission()
});
const location = ref('')
function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else {
    location.value = "Geolocation is not supported by this browser.";
  }
}

function showPosition(position) {
    location.value = "Latitude: " + position.coords.latitude +
  "<br>Longitude: " + position.coords.longitude;
}

function handlePermission() {
  navigator.permissions.query({ name: "geolocation" }).then((result) => {
    if (result.state === "granted") {
      report(result.state);
      geoBtn.style.display = "none";
    } else if (result.state === "prompt") {
      report(result.state);
      geoBtn.style.display = "none";
      navigator.geolocation.getCurrentPosition(
        revealPosition,
        positionDenied,
        geoSettings,
      );
    } else if (result.state === "denied") {
      report(result.state);
      geoBtn.style.display = "inline";
    }
    result.addEventListener("change", () => {
      report(result.state);
    });
  });
}

function report(state) {
  console.log(`Permission ${state}`);
}

function searchFood(){
    var data = {
        latlng: "21.0278,105.8342",
        keyword: "cơm",
        offset: 0,
        pageSize: 32,
        countryCode: "VN"
    };
    search(data)
}
</script>
