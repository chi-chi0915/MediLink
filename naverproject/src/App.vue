
<script>
export default {
  mounted() {
    // 네이버 지도 API 로드
    const script = document.createElement("script");
    script.src =
      "https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=lamhj6mox4";
    script.async = true;
    script.defer = true;
    document.head.appendChild(script);

    script.onload = () => {
      // 네이버 지도 생성 좌표 LatLng(37.5670135, 126.9783740)
      new window.naver.maps.Map("map", {
        center: new window.naver.maps.LatLng(37.5670135, 126.9783740),
       
        // 스크롤바 생성
        scaleControl: false,
        logoControl: false,
        mapDataControl: false,
        zoomControl: true,
        minzoom: 15

        //zoom: 15
      });
    };
  }
};
</script>

<template>
  <div id="map">
  </div>
</template>

<style scoped>
#map {
  width: 100vw;
  height: 100vh;
  background-color: lightgray; /* 배경색 추가 */
}
</style>