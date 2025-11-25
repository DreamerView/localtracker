<template>
  <div ref="globeEl" class="globe overflow-x-hidden"></div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { watchEffect } from "vue";
import Globe from "globe.gl";
import * as THREE from "three";

const props = defineProps({
        about:Object
    })

watchEffect(()=>{
    console.log("Globe")
    console.log(props.about)
})

const globeEl = ref(null);

onMounted(() => {
  const globe = Globe()(globeEl.value);

  globe
    .globeImageUrl("/earth/earth-blue-marble.jpg")
    .bumpImageUrl("/earth/earth-topology.png")
    .backgroundColor("#111");

  // CLOUDS
  const CLOUDS_IMG_URL = "/earth/clouds.png";

  new THREE.TextureLoader().load(CLOUDS_IMG_URL, (cloudsTexture) => {
    const clouds = new THREE.Mesh(
      new THREE.SphereGeometry(globe.getGlobeRadius() * 1.01, 75, 75),
      new THREE.MeshPhongMaterial({
        map: cloudsTexture,
        transparent: true,
        opacity: 0.8
      })
    );

    globe.scene().add(clouds);

    (function animate() {
      clouds.rotation.y += 0.0003;
      requestAnimationFrame(animate);
    })();
  });

  // AUTOROTATE
//   globe.controls().autoRotate = true;
//   globe.controls().autoRotateSpeed = 0.3;

  // POINT
  const cityPoint = { lat: props.about?.lat || 0, lng: props.about?.lon || 0 };
  globe.pointsData([{ ...cityPoint, size: 1, color: "rgba(255,255,255,1)" }]);
  globe.htmlElementsData([
  {
    lat: cityPoint.lat,
    lng: cityPoint.lng+7,
    html: `
      <div class="rounded-4 border px-2 py-1" style="background-color:rgba(255,255,255,0.7);">
        <div class="title fw-bold">${props.about?.city || "Unknown"}, ${props.about?.country || "Unknown"}</div>
      </div>
    `
  }
])
.htmlElement(obj => {
  const el = document.createElement("div");
  el.innerHTML = obj.html;
  el.className = "label-wrapper";
  return el;
});

  // ========== KAZAKHSTAN BORDER (ISO = KAZ) ==========
  // ========== KAZAKHSTAN BORDER (API FIXED) ==========
    fetch("https://raw.githubusercontent.com/datasets/geo-countries/master/data/countries.geojson")
    .then(res => res.json())
    .then(world => {
        let kaz = world.features.find(f => f.properties["ISO3166-1-Alpha-2"] === "KZ");

        console.log(world)

        if (kaz.geometry.type === "MultiPolygon") {
        kaz = {
            ...kaz,
            geometry: {
            type: "Polygon",
            coordinates: kaz.geometry.coordinates[0]
            }
        };
        }

        globe
        .polygonsData([kaz])
        .polygonCapColor(() => "rgba(0,150,255,0.2)")
        .polygonSideColor(() => "rgba(0,150,255,0.5)")
        .polygonStrokeColor(() => "#00aaff");
    });
    globe.camera().position.z = 280;
  // CAMERA
  setTimeout(() => {
    globe.pointOfView({ ...cityPoint, altitude: 0.7 }, 2000);
    }, 600);
});
</script>

<style scoped>
.globe {
  width: 100%;
  height: 100vh;
  background: #111;
}
</style>