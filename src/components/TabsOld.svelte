<script context="module">
    export const TABS = {};
</script>
<script>
    import { setContext, onDestroy } from 'svelte';
    import { writable } from 'svelte/store';

    const tabs = [];
    const panels = [];
    const selectedTab = writable(null);
    const selectedPanel = writable(null);

    setContext(TABS, {
        registerTab: tab => {
            tabs.push(tab);
            selectedTab.update(current => current || tab);

            onDestroy(() => {
                const i = tabs.indexOf(tab);
                tabs.splice(i, 1);
                selectedTab.update(current => current === tab ? (tabs[i] || tabs[tabs.length - 1]) : current);
            });
        },

        registerPanel: panel => {
            panels.push(panel);
            selectedPanel.update(current => current || panel);

            onDestroy(() => {
                const i = panels.indexOf(panel);
                panels.splice(i, 1);
                selectedPanel.update(current => current === panel ? (panels[i] || panels[panels.length - 1]) : current);
            });
        },

        selectTab: tab => {
            const i = tabs.indexOf(tab);
            selectedTab.set(tab);
            selectedPanel.set(panels[i]);
        },

        selectedTab,
        selectedPanel
    });
</script>
<div id="wrapper">
    <ul class="tab-wrapper">
        {#each items as item}
            <li class:active={activeTabValue === item.value} class="tab-cell">
                <span on:click={handleClick(item.value)}>{item.label}</span>
            </li>
        {/each}
    </ul>
    <div id="content">
        {#each items as item}
            {#if activeTabValue === item.value}
                <div class="box">
                    <svelte:component this={item.component}/>
                </div>
            {/if}
        {/each}
    </div>
</div>
<div class="tabs">
    <slot></slot>
</div>
<style lang="scss">
  #wrapper {
    height: 100%;
    display: flex;
    flex-direction: column;

    #content {
      overflow: scroll;
      flex: 1;

      .box {
        margin-bottom: 10px;
        padding: 40px;
        overflow: scroll;
      }
    }
  }
  ul {
    display: flex;
    flex-wrap: wrap;
    padding: 1rem;
    margin-bottom: 0;
    list-style: none;
    justify-content: space-around;

    li {
      margin-bottom: -1px;
    }
  }

  span {
    display: block;
    padding: 0.5rem 1rem;
    cursor: pointer;
    transition: all 0.1s ease-in-out;

    &:hover {
      color: grey;
    }
  }

  li.active > span {
    color: #495057;
    background-color: #fff;

    &:before {
      content: "|";
      font-weight: 700;
      color: #96AFED;
      margin-right: 4px;
    }
  }
</style>