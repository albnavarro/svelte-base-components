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

<div class="item">
    <button class="toggle" on:click={handleCick}>
        {label}
    </button>

    <div class="content" use:getNode>
        {@html content}
    </div>
</div>

<style lang="scss">

    .item {
        margin-bottom: 20px;
    }

    .toggle {
        border: 1px $grey solid;
        margin: 0;
        width: 100%;
        cursor: pointer;
    }

    .content {
        display: block;
        overflow: hidden;

        :global(img) {
            width: 100%;
        }
    }
    
</style>
