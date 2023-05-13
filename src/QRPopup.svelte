<script>
  let canvas;
  export let link = 'ton://null';

  import QRCode from '../node_modules/qrcode/';

  $: if (canvas) QRCode.toCanvas(canvas, link);
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<canvas bind:this={canvas} on:click|stopPropagation></canvas>
