<script>
    import { createEventDispatcher } from 'svelte'

    export let label = 'default label';
    export let content = 'default content';
    export let id = 0
    export let close = true;
    export let isActive = false;

    const dispatch = createEventDispatcher();

    function handleCick() {
        isActive = !isActive;
        dispatch("item-click", id);
    }

    $: activeClass = (!isActive) ? 'hide' : '';
    $: { if(close) isActive = false };


</script>


<div class="accordion__item">
    <button class="accordion__item__toggle" on:click={handleCick}>
        { label }
    </button>

    <div class="accordion__item__content { activeClass }">
        {@html content}
    </div>
</div>

<style type="text/scss">
    .accordion__item {
        margin-bottom: 20px;

        &__toggle {
            border: 1px #ccc solid;
            margin: 0;
            width: 100%;
        }

        &__content {
            display: block;

            &.hide {
                display: none;
            }

            :global(img) {
                width: 100%;
            }
        }
    }


</style>
