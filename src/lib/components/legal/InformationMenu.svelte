<script lang="ts">

    import ContactWindow from "./ContactWindow.svelte";
    import PrivacyWindow from "./PrivacyWindow.svelte";

    let contactState: boolean = false;
    const changeContactState = () => {
        dataSecurityState = false;
        contactState = !contactState;
    }

    let dataSecurityState: boolean = false;
    const changeDataSecurityState = () => {
        contactState = false;
        dataSecurityState = !dataSecurityState;
    }

</script>

<div class="left-0 bottom-0 absolute z-20 mb-2 ml-2">

    {#if contactState}
        <div>
            <ContactWindow />
        </div>
    {:else if dataSecurityState}
        <div>
            <PrivacyWindow />
        </div>
    {/if}

    <button class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-1 px-4 border border-gray-400 rounded shadow" on:click="{changeContactState}">
        <span>Contact/Impressum</span>
    </button>
    <button class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-1 px-4 border border-gray-400 rounded shadow" on:click="{changeDataSecurityState}">
        <span>Datenschutz</span>
    </button>

</div>

<style>

</style>