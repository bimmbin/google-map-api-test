<script setup>
import { onMounted, ref } from "vue";
import { Loader } from "@googlemaps/js-api-loader";

const loader = new Loader({
    apiKey: "AIzaSyASlAvQ2hTJMe-9O0PNL4RvOyjRShA-bOY",
    version: "weekly",
    libraries: ["places"],
});
const mapOptions = {
    center: {
        lat: 15.7559831,
        lng: 120.6336987,
    },
    zoom: 13,
};
const street_ref = ref();

loader
    .importLibrary("maps")
    .then(({ Map }) => {
        const geocoder = new google.maps.Geocoder();
        let map = new Map(document.getElementById("map"), mapOptions);
        let marker = new google.maps.Marker({
            map,
        });
        const autocomplete = new window.google.maps.places.Autocomplete(
            street_ref.value,
            {
                types: ["address"],
                fields: ["address_components"],
            }
        );
        google.maps.event.addListener(autocomplete, "place_changed", () => {
            const address =
                autocomplete.getPlace().address_components[0].short_name;
            geocoder.geocode({ address: address }, (results, status) => {
                if (status === "OK") {
                    map.setCenter(results[0].geometry.location);
                    marker.setPosition(results[0].geometry.location);
                    const location = results[0].geometry.location;
                    console.log("Latitude:", location.lat());
                    console.log("Longitude:", location.lng());
                } else {
                    console.error(
                        "Geocode was not successful for the following reason:",
                        status
                    );
                }
            });
        });
    })
    .catch((e) => {
        console.error("Error loading Google Maps API:", error);
    });

// function geocode(request) {
//     clear();
//     geocoder
//         .geocode(request)
//         .then((result) => {
//             const { results } = result;

//             map.setCenter(results[0].geometry.location);
//             marker.setPosition(results[0].geometry.location);
//             marker.setMap(map);
//             responseDiv.style.display = "block";
//             response.innerText = JSON.stringify(result, null, 2);
//             return results;
//         })
//         .catch((e) => {
//             alert("Geocode was not successful for the following reason: " + e);
//         });
// }
// async function initMap() {
//     await google.maps.importLibrary("places");
//     const autocomplete = new google.maps.places.Autocomplete(street_ref.value, {
//         types: ["addresss"],
//         fields: ["addresss_components"],
//     });
// }
// onMounted(() => {
//     initMap();
// });
</script>

<template>
    <div class="w-full flex justify-center items-center text-xl font-bold">
        Auto complete
    </div>

    <input type="text" ref="street_ref" class="w-full" />
    <div
        id="map"
        class="absolute top-0 left-0 w-full mt-16 bg-gray-400 h-[28rem] flex justify-center items-center text-xl text-center"
    ></div>
</template>
