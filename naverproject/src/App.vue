<template>
  <div id="map" ref="mapRef" class="map-container"></div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const mapRef = ref(null);

// 네이버 지도 API 로드 함수
const loadNaverMap = () => {
  return new Promise((resolve) => {
    if (window.naver) {
      resolve(window.naver);
      return;
    }

    const script = document.createElement("script");
    script.src =
      "https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=lamhj6mox4";
    script.async = true;
    script.defer = true;
    document.head.appendChild(script);

    script.onload = () => resolve(window.naver);
  });
};

onMounted(async () => {
  const naver = await loadNaverMap(); // 네이버 지도 API 로드 완료 후 실행

  const map = new naver.maps.Map(mapRef.value, {
    center: new naver.maps.LatLng(37.5670135, 126.9783740),
    zoom: 15,
    scaleControl: false,
    logoControl: false,
    mapDataControl: false,
    zoomControl: true,
    minZoom: 5
  });

  new naver.maps.Marker({
    position: new naver.maps.LatLng(37.3595704, 127.105399),
    map: map
  });
});
</script>

<style scoped>
#map {
  width: 100vw;
  height: 100vh;
  background-color: lightgray; /* 배경색 추가 */
}
</style>