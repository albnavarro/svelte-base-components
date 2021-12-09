
<!-- Script -->
<script>
    import AccordionItem from './AccordionItem.svelte';
    import { fade } from 'svelte/transition';

    export let multiple = false;
    export let items = [];

    let activeIndex = -1;
    let isMultiple = multiple;

    function handleCick({ detail: id }) {
        activeIndex = id;
    }
</script>


<!-- Template -->
<div in:fade>
    <div class="switch-mode">
        <button
            class="btn"
            class:active={!isMultiple}
            on:click={() => (isMultiple = false)}
        >
            only one open
        </button>
        <button
            class="btn"
            class:active={isMultiple}
            on:click={() => (isMultiple = true)}
        >
            multiple item open
        </button>
    </div>

    <div class="accordion">
        {#each items as { label, content, id }, index (id)}
            <AccordionItem
                close={activeIndex !== index && !isMultiple}
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

    .btn {
        margin-bottom: 20px;

        &.active {
            border: 2px $black solid;
        }
    }
</style>
