<svelte:head>
  <!-- <script src={"https://unpkg.com/@tonconnect/sdk@latest/dist/tonconnect-sdk.min.js"} defer /> -->
  
  <style>
    html {
      background: #f7f9fb;
      color: #232328;
      font-family: Roboto, sans-serif;
    }
  </style>
</svelte:head>

<script>
  import HorizontalLine from './HorizontalLine.svelte';
  import DeployPanel from './DeployPanel.svelte';
  import InitInput from './InitInput.svelte';
  import Header from './Header.svelte';
  import Popup from './Popup.svelte';
  
  let popup_wrapper = null;

  let is_masterchain;
  let is_testnet = false;
  let code = null;
  let data = null;
</script>

<Header bind:is_masterchain bind:is_testnet />
<HorizontalLine margin={8} color={'#b2d4fc'} />
<InitInput bind:code bind:data />
<HorizontalLine margin={8} color={'#b2d4fc'} />
<DeployPanel {code} {data} {popup_wrapper} {is_masterchain} />
<Popup bind:popup_wrapper />
