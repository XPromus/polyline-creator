<script lang="ts">

    import * as L from 'leaflet';
    import 'leaflet/dist/leaflet.css';
    import { onMount } from 'svelte';

    import { polyline } from "./lineStore";

    let map;
    let line;

    const lineTextStart: string = "[\n";
    const lineTextEnd: string = "]"
    let lineText: string;

    function lineDataToText(): string {
        let returnString: string = lineTextStart;
        for (let i = 0; i < $polyline.length; i++) {
            const position = $polyline[i];
            let positionString;
            if (i < $polyline.length - 1) {
                positionString = "  [" + position[0] + ", " + position[1] + "],\n";
            } else {
                positionString = "  [" + position[0] + ", " + position[1] + "]\n";
            }
            console.log(positionString);
            returnString += positionString;
        }
        returnString += lineTextEnd;
        return returnString;
    }

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
        return L.tileLayer('https://{s}.basemaps.cartocdn.com/rastertiles/voyager/{z}/{x}/{y}{r}.png', {
            attribution: `&copy;<a href="https://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>,&copy;<a href="https://carto.com/attributions" target="_blank">CARTO</a>`,
            subdomains: 'abcd',
            maxZoom: 25
        });
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
        lineText = lineDataToText();
        removePolyline();
    }

    function redrawPolyline() {
        if ($polyline.length > 2) {
            removePolyline();
        }
        lineText = lineDataToText();
        line = L.polyline($polyline, {color: "red"}).addTo(map);
    }

    function removePolyline() {
        line.remove(map);
    }

    onMount(async () => {

        map.on("click", function(e) {
            const position = e.latlng;
            const newPosition = [position.lat, position.lng];
            $polyline.push(newPosition);
            redrawPolyline();
        });

        lineText = lineDataToText();

    });

</script>

<svelte:window on:resize="{resizeMap}" />
<div id="map" use:mapAction />

<div id="topRight">
    <h3 id="title">
        Polyline Creator
    </h3>
    <span id="subtitle">
        A very simple Polyline Creator that (maybe) will get updates.
    </span>
    <button on:click="{resetPolyLine}" id="clearButton">
        <span style="margin-right: 5px;">Clear Line</span>
        <i class="fa-solid fa-trash-can"></i>
    </button>
    <textarea name="output" id="line-output" cols="50" rows="25" readonly>
        {lineText}
    </textarea>
</div>


<style>

    #map {
        position: absolute;
        top: 0;
        height: 100%;
        width: 100%;
    }

    #topRight {
        position: absolute;
        top: 0;
        right: 0;
        z-index: 700;
        width: 23%;
    }

    #clearButton {
        padding: 10px 10px 10px 10px;
    }

    #clearButton:hover {
        cursor: pointer;
    }

    #subtitle {
        transform: translateY(-20px);
        margin-bottom: 20px;
    }

</style>