<script lang="ts">
    export let icon: string;
    export let func;

    const buttonFunction = () => {
        func();
    }

</script>

<div>
    <button on:click={buttonFunction} class="px-2 py-1 bg-blue-400 hover:bg-blue-600 text-gray-800 font-semibold border border-blue-900 rounded shadow transition duration-150 ease-in-out">
        <i class="{icon}"></i>
    </button>
</div>

<style>

</style>