<!-- Script -->
<script>
    import * as data from './locales/components.json';
    import Test from './component/Test.svelte';
    import Accordion from './component/accordion/Accordion.svelte';

    // Import global scss and node modules css
    import NodeModulesCss from './globalStyle/NodeModulesCss.svelte';
    import GlobalStyle from './globalStyle/GlobalStyle.svelte';

    // Pairing component form string in json to real component
    const components = { Test, Accordion };

    // Active component
    let activeComponent = null;

    function handlerClick(e, id) {
        const selectedResult = data.components.find((item) => item.id === id);

        // If component exist update activeComponent
        if (selectedResult && selectedResult.component in components) {
            activeComponent = {
                component: components[selectedResult.component],
                props: selectedResult.props,
            };
        }
    }
</script>

<!-- Template -->
<main class="main">
    <div class="container global-style__test">
        {#if activeComponent}
            <svelte:component
                this={activeComponent.component}
                {...activeComponent.props}
            />
        {/if}
    </div>

    <div class="sidebar">
        <h2>Select component:</h2>
        {#each data.components as { component, props, id }, index (id)}
            <div class="item">
                <button
                    type="button"
                    class="btn"
                    on:click={(e) => handlerClick(e, id)}
                >
                    {component}
                </button>
            </div>
        {/each}
    </div>
</main>

<!-- Style -->
<style lang="scss">
    .container {
        max-width: 1440px;
        margin: 50px auto;
    }

    .sidebar {
        position: fixed;
        top: 20px;
        right: 20px;
        background: $white;
        border: 1px $grey solid;
        padding: 40px;
    }

    .item {
        margin-bottom: 20px;
    }
</style>
