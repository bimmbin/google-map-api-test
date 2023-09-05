<script setup>
import { onMounted } from "vue";
let map = null;

async function initMap() {
    await window.google.maps.importLibrary("map");
    const geocoder = new google.maps.Geocoder();
    const loc = { lat: 40.731, lng: -73.997 };

    // map = new Map(document.getElementById("map"), {
    //     center: { lat: -34.397, lng: 150.644 },
    //     zoom: 8,
    // });

    geocoder
        .geocode({ location: loc })
        .then((response) => {
            if (response.results[0]) {
                console.log(response.results[0]);
            } else {
                window.alert("No results found");
            }
        })
        .catch((e) => window.alert("Geocoder failed due to: " + e));
}

function newButton() {
    initMap();
}
</script>

<template>
    <div class="w-full flex justify-center items-center text-xl font-bold">Geocoder</div>

    <div
        id="map"
        class="absolute top-0 left-0 w-full mt-16 bg-gray-400 h-[28rem] flex justify-center items-center text-xl text-center"
    ></div>

    <span
        @click="newButton()"
        class="text-sm underline cursor-pointer capitalize"
        >set location</span
    >
</template>
