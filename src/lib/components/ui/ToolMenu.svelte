<script lang="ts">
    import { polyline } from '../../data/lineStore';
    import ColorPicker from 'svelte-awesome-color-picker';
    import ToolButton from './ToolButton.svelte';
    import ToolButtonActive from './ToolButtonActive.svelte';

    let rgb;
    let showColorPicker: boolean = false;

    let toolMode: number = 0;

    const switchPolylineCreator = () => {
        showColorPicker = !showColorPicker;
    }

    const goBack = () => {
        const line = $polyline;
        line.pop();
        polyline.set(line);
    }

    const changeToolMode = (index: number) => {
        toolMode = index;
    }

</script>

<div class="flex flex-col space-y-2 top-0 left-0 z-20 absolute ml-4 mt-4">
    <div>
        <button on:click={goBack} class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow transition duration-150 ease-in-out">
            <i class="fa-solid fa-arrow-rotate-left"></i>
        </button>
    </div>
    <div>
        <button class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow disabled:bg-gray-500 disabled:border-gray-800 transition duration-150 ease-in-out" disabled>
            <i class="fa-solid fa-arrow-rotate-right"></i>
        </button>
    </div>

    {#if toolMode == 0}
        <ToolButtonActive icon="fa-solid fa-pen" func="{() => changeToolMode(0)}"/>
        <ToolButton icon="fa-solid fa-up-down-left-right" func="{() => changeToolMode(1)}"/>
        <ToolButton icon="fa-solid fa-trash" func="{() => changeToolMode(2)}"/>
    {:else if toolMode == 1}
        <ToolButton icon="fa-solid fa-pen" func="{() => changeToolMode(0)}"/>
        <ToolButtonActive icon="fa-solid fa-up-down-left-right" func="{() => changeToolMode(1)}"/>
        <ToolButton icon="fa-solid fa-trash" func="{() => changeToolMode(2)}"/>
    {:else if toolMode == 2}
        <ToolButton icon="fa-solid fa-pen" func="{() => changeToolMode(0)}"/>
        <ToolButton icon="fa-solid fa-up-down-left-right" func="{() => changeToolMode(1)}"/>
        <ToolButtonActive icon="fa-solid fa-trash" func="{() => changeToolMode(2)}"/>
    {/if}

    <div>
        <button on:click={switchPolylineCreator} class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow transition duration-150 ease-in-out">
            <i class="fa-solid fa-palette"></i>
        </button>
        {#if showColorPicker}
            <ColorPicker bind:rgb/>
        {/if}
    </div>
</div>

<style>

</style>