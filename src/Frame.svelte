<script>
  import { onMount } from "svelte";

  export let frame;
  export let dims;
  export let width;
  export let height;

  let canvasElement;

  onMount(() => {
    const ctx = canvasElement.getContext("2d");
    const frameImageData = ctx.createImageData(dims.width, dims.height);

    frameImageData.data.set(frame);
    ctx.putImageData(frameImageData, dims.left, dims.top);
  });
</script>

<canvas bind:this={canvasElement} {width} {height} />

<style>
  canvas {
    background-image: linear-gradient(45deg, #eee 25%, transparent 25%),
      linear-gradient(-45deg, #eee 25%, transparent 25%),
      linear-gradient(45deg, transparent 75%, #eee 75%),
      linear-gradient(-45deg, transparent 75%, #eee 75%);
    background-size: 20px 20px;
    background-position: 0 0, 0 10px, 10px -10px, -10px 0px;
    width: 100%;
  }
</style>
