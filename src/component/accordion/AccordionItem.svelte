<script>
    import { createEventDispatcher } from 'svelte';
    import { onMount } from 'svelte';
    import { tweened } from 'svelte/motion';
    import { cubicOut } from 'svelte/easing';

    export let label = 'default label';
    export let content = 'default content';
    export let id = 0;
    export let close = true;
    export let isActive = false;

    const dispatch = createEventDispatcher();

    // Get content node
    let contentNode;
    const getNode = (node) => (contentNode = node);

    // Set up tween
    const height = tweened(0, {
        duration: 400,
        easing: cubicOut,
    });

    // Close contnt
    function closeContent() {
        height.set(0);
    }

    // Open content
    function opemContent() {
        contentNode.style.height = 'auto';
        const contentHeight = contentNode.clientHeight;
        contentNode.style.height = '0px';
        height.set(contentHeight).then(() => {
            contentNode.style.height = 'auto';
        });
    }

    // Toggle click
    function handleCick() {
        isActive = !isActive;
        dispatch('item-click', id);
    }

    // Reactive action
    $: {
        !isActive ? closeContent() : opemContent();
    }
    $: {
        if (close) isActive = false;
    }

    onMount(() => {
        // Apply tween
        const unsubscribe = height.subscribe((val) => {
            if (contentNode) contentNode.style.height = `${val}px`;
        });

        return unsubscribe;
    });
</script>

<div class="accordion__item">
    <button class="accordion__item__toggle" on:click={handleCick}>
        {label}
    </button>

    <div class="accordion__item__content" use:getNode>
        {@html content}
    </div>
</div>

<style lang="scss">
    .accordion__item {
        margin-bottom: 20px;

        &__toggle {
            border: 1px #ccc solid;
            margin: 0;
            width: 100%;
            cursor: pointer;
        }

        &__content {
            display: block;
            overflow: hidden;

            :global(img) {
                width: 100%;
            }
        }
    }
</style>
