<script lang="ts">
    import type Calendarium from "src/main";
    import Calendar from "./Calendar.svelte";
    import CalendariumView from "../view";
    import { writable } from "svelte/store";
    import { type CalendarStore } from "src/stores/calendar.store";
    import { type ViewParent } from "../view.types";
    import { setTypedContext } from "../view.utils";

    export let store: CalendarStore | null;
    export let plugin: Calendarium;
    export let view: ViewParent;
    export let full: boolean;
    setTypedContext("plugin", plugin);
    setTypedContext("view", view);
    const fullStore = writable(full);
    $: $fullStore = full;

    setTypedContext("full", fullStore);
    $: {
        if (store) {
            setTypedContext("store", writable(store));
            setTypedContext(
                "ephemeralStore",
                writable(store.getEphemeralStore(view.id)),
            );
        }
    }
</script>

{#if !store}
    <p>No calendars created! Create one in settings to get started.</p>
{:else}
    <div class="calendar-container calendarium">
        {#key store}
            <Calendar />
        {/key}
    </div>
{/if}

<style scoped>
    .calendar-container {
        padding: 0 8px 20px 8px;
        background: inherit;
        display: flex;
        flex-flow: column;
        overflow: auto;
        height: 100%;
    }
</style>
