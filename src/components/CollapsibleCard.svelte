<script>
    import collapse from 'svelte-collapse';

    export let open = false;

    let expandIcon;

    function onToggle() {
        open = !open;
        let deg = open ? 180 : 0;
        expandIcon.style.transform = `rotate(${deg}deg)`;
    }
</script>
<div class="card" class:open aria-expanded={open}>
    <div class="collapsible-header">
        <div class="container">
            <slot name="header"/>
        </div>
        <button class="btn-expand" on:click={onToggle}><i bind:this={expandIcon} class="fa-solid fa-caret-down"></i></button>
    </div>
    <div class="collapsible-body" use:collapse={{open}}>
        <slot name="body"/>
    </div>
</div>
<style lang="scss">
  .collapsible-header {
    display: flex;

    .container {
      align-self: start;
    }

    .btn-expand {
      background: none;
      align-self: flex-end;
      flex: 1;
      margin: auto 1rem auto 0;

      i {
        transition: ease-in-out 0.2s;
        float: right;
        right: 1.5rem;
        color: #383838;
      }
    }
  }
</style>