<script>
  export let is_masterchain;
  export let is_testnet;
  export let as_plugin;

  import TonWeb from '../node_modules/tonweb/dist/tonweb.js';
  import TWM from '../node_modules/tonweb-mnemonic/dist/web/index.js';

  let mnemonic;  // automatically encapsulated in component
  let mnemonic_valid;
  let address_list = {};

  console.log('as_plugin:', as_plugin);

  async function fill_address_list(_mnemonic) {
    if (!_mnemonic) return;

    mnemonic_valid = await TWM.validateMnemonic(mnemonic.split(' '));

    // EVEN IF MNEMONIC IS INVALID, WE MUST PROCEED TO GENERATING ADDRESSES
    const kp = await TWM.mnemonicToKeyPair(mnemonic.split(' '));
    
    for (let [wallet_name, wallet_constructor] of Object.entries(TonWeb.Wallets.all)) {
      let wallet = new wallet_constructor(null, {publicKey: kp.publicKey});
      address_list[wallet_name] = (await wallet.getAddress()).toString(true, true, true);
    }
  }

  $: fill_address_list(mnemonic);
</script>

<style>
  .root {
    background-color: #fff;
    padding: 8px;
    width: 600px;
    display: flex;
    flex-direction: column;
  }
  textarea {
    resize: vertical;
    margin: 8px;
  }
  #wallets {
    margin-top: 8px;
    margin-bottom: 8px;
  }
</style>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="root" on:click|stopPropagation>
  <textarea placeholder="24 mnemonic words" bind:value={mnemonic}></textarea>

  {#if !mnemonic_valid}
    <div>Warning: invalid mnemonic</div>
  {/if}

  <div id="wallets">
    {#each Object.entries(address_list) as [wallet_name, addr]}
      <div>{wallet_name}: {addr}</div>
    {/each}
  </div>

  <div>
    Deploying: mc:{is_masterchain}, ts:{is_testnet}
  </div>
</div>
