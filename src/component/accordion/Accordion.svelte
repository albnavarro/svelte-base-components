
<!-- Script -->
<script>
    import AccordionItem from './AccordionItem.svelte';
    import { fade } from 'svelte/transition';

    export let multiple = false;
    export let items = [];

    let activeIndex = -1;

    function handleCick({ detail: id }) {
        activeIndex = id;
    }
</script>


<!-- Template -->
<div in:fade>

    <!-- test switch mode -->
    <div>
        <button
            class:active={!multiple}
            on:click={() => (multiple = false)}
        >
            Single mode
        </button>
        <button
            class:active={multiple}
            on:click={() => (multiple = true)}
        >
            Multiple mode
        </button>
    </div>

    <!-- Accordion -->
    <div class="accordion">
        {#each items as { label, content, id }, index (id)}
            <AccordionItem
                close={activeIndex !== index && !multiple}
                {label}
                {content}
                id={index}
                on:item-click={handleCick}
            />
        {/each}
    </div>
</div>

<!-- Style -->
<style lang="scss">
    .accordion {
        max-width: 300px;
    }

    button {
        margin-bottom: 20px;

        &.active {
            border: 2px $black solid;
        }
    }
</style>
