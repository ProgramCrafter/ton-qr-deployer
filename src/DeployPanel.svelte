<style>
  .panel {
    display: flex;
    margin: 8px;
    margin-top: 0;
  }
  .panel>* {
    margin: 8px 8px;
  }
  button {
    padding: 8px 24px;
    min-height: 50px;
    border-radius: 16px;
    background-color: #b2d4fc;
    color: #232328;
    border: #f7f9fb 1px solid;
    align-items: center;
  }
  button:disabled {
    background-color: #aaa;
  }
</style>

<script>
  import MnemonicPopup from './MnemonicPopup.svelte';
  import QrPopup from './QRPopup.svelte';

  import TonWeb from '../node_modules/tonweb/dist/tonweb.js';
  
  export let code;
  export let data;
  export let popup_wrapper;
  export let is_masterchain;

  function to_base64(bytes) {
    return TonWeb.utils.bytesToBase64(bytes).replaceAll('+', '-').replaceAll('/', '_');
  }

  function get_state_init() {
    let ec = new TonWeb.boc.Cell();
    let init = new TonWeb.boc.Cell();
    init.bits.writeUint(6, 5);
    init.refs.push(code ?? ec);
    init.refs.push(data ?? ec);
    return init;
  }

  async function get_contract_addr(state_init_cell) {
    let init_hash = TonWeb.utils.bytesToHex(await state_init_cell.hash());
    let workchain = is_masterchain ? '-1:' : '0:';
    return (new TonWeb.Address(workchain + init_hash)).toString(true, true, true);
  }


  function unsigned_tx_request(addr, boc) {
    return JSON.stringify({
      version: '0',
       body: {
        type: 'sign-raw-payload',
        expires_sec: Math.ceil(new Date() / 1000 + 86400),
        response_options: {broadcast: true},
        params: {
          messages: [{
            address: addr,
            stateInit: TonWeb.utils.bytesToBase64(boc),
            amount: '90000000'
          }]
        }
      }
    });
  }

  async function popup_ton_qr() {
    const init_cell = get_state_init();
    const addr = await get_contract_addr(init_cell);
    let link = 'ton://transfer/' + addr + '?init=' + to_base64(await init_cell.toBoc());

    console.log('Not shortened:', link);

    if (link.length >= 256) {
      let stored_shortened = window.localStorage.getItem('short:' + addr);
      if (!stored_shortened) {
        let shorten_rq = await fetch('https://jsonblob.com/api/jsonBlob', {
          method: 'POST',
          body: unsigned_tx_request(addr, await init_cell.toBoc()),
          headers: {'Content-Type': 'application/json'}
        });
        let nonwrapped_link = shorten_rq.headers.get('Location').slice('http://'.length);
        link = 'https://app.tonkeeper.com/v1/txrequest-url/' + nonwrapped_link;
        
        window.localStorage.setItem('short:' + addr, link);
      } else {
        link = stored_shortened;
      }
    }
    
    console.log('Using:', link);

    let qr = new QrPopup({target: popup_wrapper});
    qr.$set({link});
  }

  async function popup_mnemonic_default() {
    // const init_cell = get_state_init();
    // const addr = await get_contract_addr(init_cell);
    
    let popup = new MnemonicPopup({
      target: popup_wrapper,
      props: {as_plugin: false}
    });
  }
</script>

<div class="panel">
  <button on:click={popup_ton_qr}>Deploy with ton:// link</button>
  <button disabled>Deploy with TON Connect 2</button>
  <button disabled>Deploy with mnemonic</button>
  <button disabled>Deploy&nbsp;<b>as plugin</b><br>
                   with mnemonic (v4r2 only)<br>
                   <b>(CAN TAKE ALL FUNDS)</b></button>
</div>
