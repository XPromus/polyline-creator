<script lang="ts">

    import * as L from 'leaflet';
    import 'leaflet/dist/leaflet.css';
    import { onMount } from 'svelte';
    import { polyline } from "./lineStore";
    import { fade } from 'svelte/transition';

    import Contact from "./Contact.svelte";
    import Privacy from "./Privacy.svelte";

    let map;
    let line;

    const lineTextStart: string = "[\n";
    const lineTextEnd: string = "]"
    let lineText: string;

    function copyTextToClipboard() {
        navigator.clipboard.writeText(lineText);
    }

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

    function copyPolyLineText() {

    }

    let contactState: boolean = false;
    function changeContactState() {
        dataSecurityState = false;
        contactState = !contactState;
    }

    let dataSecurityState: boolean = false;
    function changeDataSecurityState() {
        contactState = false;
        dataSecurityState = !dataSecurityState;
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
    <div id="textBox">
        <button on:click="{resetPolyLine}" class="button">
            <span style="margin-right: 5px;">Clear Line</span>
            <i class="fa-solid fa-trash-can"></i>
        </button>
        <button on:click="{copyTextToClipboard}" class="button">
            <span>Copy Text</span>
        </button>
        <textarea name="output" id="line-output" cols="50" rows="25" readonly>
            {lineText}
        </textarea>
    </div>
</div>

<div id="bottomLeft">
    {#if contactState}
        <div>
            <Contact />
        </div>
    {:else if dataSecurityState}
        <div>
            <Privacy />
        </div>
    {/if}
    <button on:click="{changeContactState}">
        <span>Contact/Impressum</span>
    </button>
    <button on:click="{changeDataSecurityState}">
        <span>Datenschutz</span>
    </button>
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

    #bottomLeft {
        left: 0;
        bottom: 0;
        position: absolute;
        z-index: 700;
        margin-bottom: 5px;
        margin-left: 5px;
    }

    #textBox {
        margin-top: 25px;
    }

    .button {
        padding: 10px 10px 10px 10px;
    }

    #subtitle {
        transform: translateY(-20px);
        margin-bottom: 20px;
    }

    button:hover {
        cursor: pointer;
    }

</style>