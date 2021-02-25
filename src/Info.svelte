<script>
  import Pair from './Pair.svelte'

  export let dims
  export let disposalType
  export let delay
</script>


<div class="core">
  <div class="pair">
    <Pair label='disposal type' value={disposalType} />
    <Pair label='delay' value={delay} />
  </div>

  <div class="pair">
    <Pair label='width' value={dims.width} />
    <Pair label='height' value={dims.height} />
  </div>

  <div class="pair">
    <Pair label='left' value={dims.left} />
    <Pair label='top' value={dims.top} />
  </div>
</div>

<style>
  .core {
    display: flex;
    width: 100%;
    justify-content: space-between;
  }

  .pair {
    width: 140px;
    display: flex;
    flex-direction: column;
  }
</style>