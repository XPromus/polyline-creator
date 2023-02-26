<script lang="ts">
    import { polyline } from '../../data/lineStore';
    import ColorPicker from 'svelte-awesome-color-picker';

    let rgb;
    let showColorPicker: boolean = false;

    const switchPolylineCreator = () => {
        showColorPicker = !showColorPicker;
    }

    const goBack = () => {
        const line = $polyline;
        line.pop();
        polyline.set(line);
    }

</script>

<div class="flex flex-col space-y-2 top-0 left-0 z-20 absolute ml-4 mt-4">
    <div>
        <button on:click={goBack} class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow">
            <i class="fa-solid fa-arrow-rotate-left"></i>
        </button>
    </div>
    <div>
        <button class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow disabled:bg-gray-500 disabled:border-gray-800" disabled>
            <i class="fa-solid fa-arrow-rotate-right"></i>
        </button>
    </div>
    <div>
        <button class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow">
            <i class="fa-solid fa-pen"></i>
        </button>
    </div>
    <div>
        <button class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow">
            <i class="fa-solid fa-up-down-left-right"></i>
        </button>
    </div>
    <div>
        <button class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow">
            <i class="fa-solid fa-trash"></i>
        </button>
    </div>
    <div>
        <button on:click={switchPolylineCreator} class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow">
            <i class="fa-solid fa-palette"></i>
        </button>
        {#if showColorPicker}
            <ColorPicker bind:rgb/>
        {/if}
    </div>
</div>

<style>

</style>