<script>
  export let is_testnet = true;
  export let is_masterchain = false;
</script>

<style>
  header {
    display: flex;
    margin: 8px;
    margin-top: 0;
  }
  .icon {
    width: 24px;
    height: 24px;
    padding: 4px; padding-left: 0;
    margin-right: 8px;
    display: inline-block;
    vertical-align: middle;
  }
  .greyout {
    filter: grayscale(1);
    transform: scaleY(-1);
  }
  header>* {
    margin: 8px 8px;
  }
  button, .rounded {
    padding: 8px 24px;
    border-radius: 16px;
    background-color: #b2d4fc80;
    color: #232328;
    border: #ccc 1px solid;
    display: flex; align-items: center;
  }
  span {
    display: flex; align-items: center;
    color: #232328;
  }
  .fill-space {
    flex-grow: 1;
  }
  .warn-border {
    border-color: orange;
  }
</style>

<header>
  <div>
    <div class="icon"></div>
    <span style="height: 100%; display: inline-flex; font-weight: 800; font-size: 18px;">
      WebDeployer
    </span>
  </div>
  <div class="fill-space" />
  <button on:click={() => {is_testnet = !is_testnet;}}>
    <img class="icon" class:greyout={is_testnet} src="https://ton.org/download/ton_symbol.svg" alt="TON logo" />
    <span>Now: {is_testnet ? 'testnet' : 'mainnet'}</span>
  </button>
  <button on:click={() => {is_masterchain = !is_masterchain;}}>
    <span>{is_masterchain ? 'masterchain (-1)' : 'basechain (0)'}</span>
  </button>
  <div class="rounded">
    <a href="https://ratingers.pythonanywhere.com/">Author & other tools</a>&nbsp;
    <a href="https://github.com/ProgramCrafter/ton-qr-deployer">GitHub</a>
  </div>
</header>
