<script>
    import * as data from './locales/components.json';
    import Test from './component/Test.svelte';
    import Accordion from './component/accordion/Accordion.svelte';

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

<main class="main">
    <div class="main__container">
        {#if activeComponent}
            <svelte:component
                this={activeComponent.component}
                {...activeComponent.props}
            />
        {/if}
    </div>

    <div class="main__sidebar">
        <h2>Select component:</h2>
        {#each data.components as { component, props, id }, index (id)}
            <div class="main__sidebar__item">
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

<style type="text/scss">
    .main {
        &__container {
            max-width: 1440px;
            margin: 50px auto;
        }

        &__sidebar {
            position: fixed;
            top: 20px;
            right: 20px;
            background: white;
            border: 1px #ccc solid;
            padding: 40px;
        }
    }
</style>
