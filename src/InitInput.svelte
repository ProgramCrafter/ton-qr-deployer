<script>
  import TonWeb from '../node_modules/tonweb/dist/tonweb.js';

  function boc_to_cell(text) {
    if (!text) return null;

    let bytes = null;
    
    try {
      bytes = TonWeb.utils.hexToBytes(text);
    } catch (e) {
      try {
        bytes = TonWeb.utils.base64ToBytes(text);
      } catch (e) {
        throw new Error('Text is neither hex nor base64');
      }
    }
    
    return TonWeb.boc.Cell.oneFromBoc(bytes);
  }

  const default_contract_code = {
    'Empty cell': '',
    'Wallet v3r2': 'te6cckEBAQEAcQAA3v8AIN0gggFMl7ohggEznLqxn3Gw7UTQ0x/THzHXC//jBOCk8mCDCNcYINMf0x/TH/gjE7vyY+1E0NMf0x/T/9FRMrryoVFEuvKiBPkBVBBV+RDyo/gAkyDXSpbTB9QC+wDo0QGkyMsfyx/L/8ntVBC9ba0=',
    'Wallet v4r2': 'te6cckECFAEAAtQAART/APSkE/S88sgLAQIBIAIDAgFIBAUE+PKDCNcYINMf0x/THwL4I7vyZO1E0NMf0x/T//QE0VFDuvKhUVG68qIF+QFUEGT5EPKj+AAkpMjLH1JAyx9SMMv/UhD0AMntVPgPAdMHIcAAn2xRkyDXSpbTB9QC+wDoMOAhwAHjACHAAuMAAcADkTDjDQOkyMsfEssfy/8QERITAubQAdDTAyFxsJJfBOAi10nBIJJfBOAC0x8hghBwbHVnvSKCEGRzdHK9sJJfBeAD+kAwIPpEAcjKB8v/ydDtRNCBAUDXIfQEMFyBAQj0Cm+hMbOSXwfgBdM/yCWCEHBsdWe6kjgw4w0DghBkc3RyupJfBuMNBgcCASAICQB4AfoA9AQw+CdvIjBQCqEhvvLgUIIQcGx1Z4MesXCAGFAEywUmzxZY+gIZ9ADLaRfLH1Jgyz8gyYBA+wAGAIpQBIEBCPRZMO1E0IEBQNcgyAHPFvQAye1UAXKwjiOCEGRzdHKDHrFwgBhQBcsFUAPPFiP6AhPLassfyz/JgED7AJJfA+ICASAKCwBZvSQrb2omhAgKBrkPoCGEcNQICEekk30pkQzmkD6f+YN4EoAbeBAUiYcVnzGEAgFYDA0AEbjJftRNDXCx+AA9sp37UTQgQFA1yH0BDACyMoHy//J0AGBAQj0Cm+hMYAIBIA4PABmtznaiaEAga5Drhf/AABmvHfaiaEAQa5DrhY/AAG7SB/oA1NQi+QAFyMoHFcv/ydB3dIAYyMsFywIizxZQBfoCFMtrEszMyXP7AMhAFIEBCPRR8qcCAHCBAQjXGPoA0z/IVCBHgQEI9FHyp4IQbm90ZXB0gBjIywXLAlAGzxZQBPoCFMtqEssfyz/Jc/sAAgBsgQEI1xj6ANM/MFIkgQEI9Fnyp4IQZHN0cnB0gBjIywXLAlAFzxZQA/oCE8tqyx8Syz/Jc/sAAAr0AMntVGliJeU='
  };
  const default_contract_data = {
    'Empty cell': ''
  };

  export let code = null;
  export let data = null;

  let code_text = '';
  let data_text = '';

  let code_error = '';
  let data_error = '';

  $: try {code = boc_to_cell(code_text); code_error = '';} catch (e) {code_error = '' + e;}
  $: try {data = boc_to_cell(data_text); data_error = '';} catch (e) {data_error = '' + e;}
</script>

<style>
  #code {
    display: flex;
    margin: 8px;
  }
  #data {
    display: flex;
    margin: 8px;
  }
  textarea {
    width: calc(50% - 8px);
    margin-right: 16px;
    min-height: 160px;
  }
</style>

<div id="code">
  <textarea bind:value={code_text} placeholder='Contract code BOC'></textarea>
  <div>
    {#each Object.entries(default_contract_code) as [display, boc]}
    <button on:click={() => {code_text = boc;}}>{display}</button>
    {/each}
    <span>{code_error || 'Code BOC parsed successfully'}</span>
  </div>
</div>
<div id="data">
  <textarea bind:value={data_text} placeholder='Contract data BOC'></textarea>
  <div>
    {#each Object.entries(default_contract_data) as [display, boc]}
    <button on:click={() => {data_text = boc;}}>{display}</button>
    {/each}
    <span>{data_error || 'Data BOC parsed successfully'}</span>
  </div>
</div>
