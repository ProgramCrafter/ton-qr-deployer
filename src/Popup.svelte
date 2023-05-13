<script>
  export let popup_wrapper;
</script>

<style>
  .popup {
    position: fixed;
    left: 0; right: 0; top: 0; bottom: 0;
    background-color: #3338;

    display: flex;
    align-items: center;
    justify-content: center;
  }
  .popup:empty {
    display: none; visibility: hidden;
  }
</style>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="popup"
  bind:this={popup_wrapper}
  on:click={(e) => {e.currentTarget.children[0].remove();}}></div>
