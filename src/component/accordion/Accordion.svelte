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

<div in:fade>
    <div class="switch-mode">
        <button
            class="btn btn--multiple-off {!isMultiple ? 'active' : ''}"
            on:click={() => (isMultiple = false)}
        >
            only one open
        </button>
        <button
            class="btn btn--multiple-on {isMultiple ? 'active' : ''}"
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


<style lang="scss">
    .accordion {
        max-width: 300px;
    }

    .btn {
        &.active {
            border: 2px black solid;
        }
    }
</style>
