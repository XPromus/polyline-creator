<script lang="ts">
    export let icon: string;
    export let func;

    const buttonFunction = () => {
        func();
    }

</script>

<div>
    <button on:click={buttonFunction} class="px-2 py-1 bg-white hover:bg-gray-100 text-gray-800 font-semibold border border-gray-400 rounded shadow transition duration-150 ease-in-out">
        <i class="{icon}"></i>
    </button>
</div>

<style>

</style>