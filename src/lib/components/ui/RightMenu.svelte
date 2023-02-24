<script lang="ts">

    import { polyline } from "../../data/lineStore";

    export let resetPolyLine;
    
    const lineTextStart: string = "[\n";
    const lineTextEnd: string = "]"

    const lineDataToText = (input: string[]): string => {
        let returnString: string = lineTextStart;
        for (let i = 0; i < input.length; i++) {
            const position = input[i];
            let positionString;
            if (i < input.length - 1) {
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


    let lineText: string;
    polyline.subscribe(value => {
        lineText = lineDataToText(value);
    })

    const copyTextToClipboard = () => {
        navigator.clipboard.writeText(lineText)
    }

</script>

<div class="absolute z-20 top-0 right-0 w-1/4 mt-4 mr-4">
    <h3 class="text-lg font-bold">Polyline Creator</h3>
    <span>A very simple Polyline Creator that (maybe) will get updates.</span>
    <div class="mt-6">
        <div class="mb-2">
            <button on:click="{resetPolyLine}" class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-1 px-4 border border-gray-400 rounded shadow">
                <span style="margin-right: 5px;">Clear Line</span>
                <i class="fa-solid fa-trash-can"></i>
            </button>
            <button on:click="{copyTextToClipboard}" class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-1 px-4 border border-gray-400 rounded shadow">
                <span>Copy Text</span>
            </button>
        </div>
        <textarea class="block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500" name="output" id="line-output" cols="50" rows="25" readonly>
            {lineText}
        </textarea>
    </div>
</div>

<style>
    
</style>