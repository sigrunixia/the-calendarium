<script lang="ts">
    import { getTypedContext } from "../../view.utils";
    import Dot from "./Dot.svelte";
    import type { CalEvent, EventLike } from "src/@types";

    export let events: EventLike[] = [];

    const store = getTypedContext("store");
    const { categories } = $store;

    const color = (event: EventLike) => {
        return $categories.find((c) => c.id == event.category)?.color;
    };
    $: overflow = Math.max(events.length - 2, 0);
</script>

<div class="dots-container">
    <div class="dot-container centered">
        {#each events.slice(0, 2) as event}
            <Dot color={color(event)} />
        {/each}
        {#if overflow > 0}
            <div class="overflow">
                <span>+</span>
            </div>
        {/if}
    </div>
</div>

<style>
    .dots-container {
        width: 100%;
    }
    .dot-container {
        display: flex;
        flex-flow: row nowrap;
        gap: 2px;
        margin: auto;
        line-height: 6px;
        min-height: 6px;
    }
    .centered {
        justify-content: center;
        align-items: center;
    }
    .overflow {
        color: var(--text-muted);
        font-size: xx-small;
        display: flex;
        justify-content: flex-end;
        line-height: 1.25;
    }
</style>
