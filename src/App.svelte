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

    // Inizialize Active component
    let activeComponent = {
        component: null,
        id: -1
    };

    function handlerClick(e, id) {
        // Get component from data
        const selectedResult = data.components.find((item) => item.id === id);

        // Secure check
        // const isTheSameComponent = activeComponent?.id === id;
        // const componentExist = selectedResult?.component in components;

        //Atom make trouble with optional chaining in template arggrr ....
        const isTheSameComponent = 'id' in activeComponent && activeComponent.id === id;
        const componentExist = selectedResult.component in components;

        //Set active component
        if (componentExist && !isTheSameComponent) {
            activeComponent = {
                component: components[selectedResult.component],
                props: selectedResult.props,
                id
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
