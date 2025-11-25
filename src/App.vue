<template>
  <div class="globe-wrapper position-relative bg-black">

    <!-- OVERLAY -->
    <div class="overlay">
      <Main :about="about" />
    </div>

    <!-- GLOBE -->
    <component
      :is="about?.loading ? null : GlobeView"
      :about="about"
    />
  </div>
</template>

<script setup>
  import GlobeView from './components/GlobeView.vue';
  import Main from './components/Main.vue';
  import {onMounted,reactive} from 'vue';
  const about = reactive(
    {
      city:null,
      country:null,
      capital:null,
      country_code:null,
      continent:null,
      lat:null,
      lon:null,
      connection:null,
      current_time:null,
      timezone:null,
      loading:true,
      error:false,
      errorText:""
    });
  const fetchData = async() => {
    try {
      about.loading = true;
      about.error = false;
      const req = await fetch("https://ipwho.is");
      if(!req.ok) throw new Error("Fetching error");
      const res = await req.json();
      about.capital = res.capital;
      about.city = res.city;
      about.connection = res.connection;
      about.continent = res.continent;
      about.country = res.country;
      about.country_code = res.country_code
      about.ip = res.ip;
      about.lat = res.latitude;
      about.lon = res.longitude;
      about.current_time = new Date(res.timezone.current_time);
      about.timezone = res.timezone
      console.log(about)
    } catch(error) {
      about.error = true;
      about.errorText = error;
      console.error("Error:", error);
    } finally {
      about.loading = false;
    }
  }

  onMounted(fetchData)
</script>

<style scoped>
  .globe-wrapper {
  position: relative;
  width: 100%;
  height: 100dvh;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100dvh;
  z-index: 9999;       /* ❤️ ЧТО СПАСАЕТ */
  overflow-y: auto;
}

</style>
