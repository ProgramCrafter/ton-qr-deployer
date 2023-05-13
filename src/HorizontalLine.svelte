<script>
  export let margin = 4;
  export let color = 'grey';
</script>

<style>
  div {
    height: 1px;
    border-top: var(--color) 1px solid;
    margin: var(--margin);
  }
</style>

<div style="--margin: {margin}px; --color: {color};"></div>
