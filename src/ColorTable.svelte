<script>
  export let colors 
  export let exists
</script>

<div class='label'>local color table</div>
{#if exists}
  <div class="box">
    {#each colors as color}
      <div class="color" style="--color: rgb({color[0]}, {color[1]}, {color[2]})"></div>
    {/each}
  </div>
{:else}
  —
{/if}

<style>
  .label {
    padding: 8px 0;
    color: #757575;
  }

  .box {
    display: grid;
    grid-template-columns: repeat(32, 16px);
    grid-gap: 1px;
  }

  .color {
    background-color: var(--color);

    width: 16px;
    height: 16px;

    border-radius: 2px
  }
</style>
