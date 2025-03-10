<template>
  <div id="map" ref="mapRef" class="map-container"></div>
</template>

<script setup>
/* eslint-disable no-unused-vars */
import { ref, onMounted } from "vue";

const mapRef = ref(null);

// 여러 개의 마커 좌표 배열
const markerData = [
  { lat: 37.5670135, lng: 126.9783740 }, // 서울
  { lat: 37.3595704, lng: 127.105399 },  // 분당
  { lat: 35.1795543, lng: 129.0756416 }, // 부산
];

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
    zoom: 15,     // 초기 줌
    minZoom: 5,   // 최소 줌
    scaleControl: false,
    logoControl: false,
    mapDataControl: false,
    zoomControl: true,
  });

// 마커 객체를 배열로 저장
const markers = markerData.map((data) => {
  return new naver.maps.Marker({
    position: new naver.maps.LatLng(data.lat, data.lng),
    map: map,
    title: data.title
  });
});

// 마커 보일 영억을 지정
var HOME_PATH = window.HOME_PATH || '.';
var MARKER_SPRITE_X_OFFSET = 29,
    MARKER_SPRITE_Y_OFFSET = 50,
    MARKER_SPRITE_POSITION = {
        "A0": [0, 0],
        "B0": [MARKER_SPRITE_X_OFFSET, 0],
        "C0": [MARKER_SPRITE_X_OFFSET*2, 0],
        "D0": [MARKER_SPRITE_X_OFFSET*3, 0],
        "E0": [MARKER_SPRITE_X_OFFSET*4, 0],
        "F0": [MARKER_SPRITE_X_OFFSET*5, 0],
        "G0": [MARKER_SPRITE_X_OFFSET*6, 0],
        "H0": [MARKER_SPRITE_X_OFFSET*7, 0],
        "I0": [MARKER_SPRITE_X_OFFSET*8, 0],

        "A1": [0, MARKER_SPRITE_Y_OFFSET],
        "B1": [MARKER_SPRITE_X_OFFSET, MARKER_SPRITE_Y_OFFSET],
        "C1": [MARKER_SPRITE_X_OFFSET*2, MARKER_SPRITE_Y_OFFSET],
        "D1": [MARKER_SPRITE_X_OFFSET*3, MARKER_SPRITE_Y_OFFSET],
        "E1": [MARKER_SPRITE_X_OFFSET*4, MARKER_SPRITE_Y_OFFSET],
        "F1": [MARKER_SPRITE_X_OFFSET*5, MARKER_SPRITE_Y_OFFSET],
        "G1": [MARKER_SPRITE_X_OFFSET*6, MARKER_SPRITE_Y_OFFSET],
        "H1": [MARKER_SPRITE_X_OFFSET*7, MARKER_SPRITE_Y_OFFSET],
        "I1": [MARKER_SPRITE_X_OFFSET*8, MARKER_SPRITE_Y_OFFSET],

        "A2": [0, MARKER_SPRITE_Y_OFFSET*2],
        "B2": [MARKER_SPRITE_X_OFFSET, MARKER_SPRITE_Y_OFFSET*2],
        "C2": [MARKER_SPRITE_X_OFFSET*2, MARKER_SPRITE_Y_OFFSET*2],
        "D2": [MARKER_SPRITE_X_OFFSET*3, MARKER_SPRITE_Y_OFFSET*2],
        "E2": [MARKER_SPRITE_X_OFFSET*4, MARKER_SPRITE_Y_OFFSET*2],
        "F2": [MARKER_SPRITE_X_OFFSET*5, MARKER_SPRITE_Y_OFFSET*2],
        "G2": [MARKER_SPRITE_X_OFFSET*6, MARKER_SPRITE_Y_OFFSET*2],
        "H2": [MARKER_SPRITE_X_OFFSET*7, MARKER_SPRITE_Y_OFFSET*2],
        "I2": [MARKER_SPRITE_X_OFFSET*8, MARKER_SPRITE_Y_OFFSET*2]
    };

// 지도의 경계를 구함
var bounds = map.getBounds(),
southWest = bounds.getSW(),
northEast = bounds.getNE(),
lngSpan = northEast.lng() - southWest.lng(),
latSpan = northEast.lat() - southWest.lat();

naver.maps.Event.addListener(map, 'idle', function () {
    updateMarkers(map, markers);
});

function updateMarkers(map, markers) {
    var mapBounds = map.getBounds();
    for (const marker of markers) {
        if (mapBounds.hasLatLng(marker.getPosition())) {
            showMarker(map, marker);
        } else {
            hideMarker(map, marker);
        }
    }
}

function showMarker(map, marker) {
    if (marker.getMap()) return;
    marker.setMap(map);
}

function hideMarker(map, marker) {
    if (!marker.getMap()) return;
    marker.setMap(null);
}


});
</script>

<style scoped>
#map {
  width: 100vw;
  height: 100vh;
  background-color: lightgray; /* 배경색 추가 */
}
</style>