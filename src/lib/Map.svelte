<script lang="ts">

    import * as L from 'leaflet';
    import 'leaflet/dist/leaflet.css';
    import { onMount } from 'svelte';
    import RightMenu from './components/ui/RightMenu.svelte';
    import { polyline } from "./data/lineStore";

    let map;
    let line;

    const startCoords = {
        lati: 36.065758,
        long: 139.522169,
        zoom: 8
    };

    function createMap(container) {
        const m = L.map(container).setView([startCoords.lati, startCoords.long], startCoords.zoom);
        m.addLayer(defaultMapLayer());
        return m;
    }

    function defaultMapLayer() {
        return L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
            maxZoom: 25
        })
    }

    function mapAction(container) {
        map = createMap(container);
        return {
            destroy: () => {
                map.remove();
            }
        }
    }

    function resizeMap() {
        if(map) {
            map.invalidateSize();
        }
    }

    function resetPolyLine() {
        $polyline = Array(0);
        removePolyline();
    }

    function redrawPolyline() {
        if ($polyline.length > 2) {
            removePolyline();
        }
        line = L.polyline($polyline, {color: "red"}).addTo(map);
    }

    function removePolyline() {
        line.remove(map);
    }

    onMount(async () => {
        map.on("click", function(e) {
            const position = e.latlng;
            const newPosition = [position.lat, position.lng];
            polyline.update(n => [...n, newPosition]);
            redrawPolyline();
        });
    });

</script>

<svelte:window on:resize="{resizeMap}" />
<div class="absolute z-10 top-0 h-full w-full" use:mapAction />
<RightMenu resetPolyLine="{resetPolyLine}" />

<style>

</style>