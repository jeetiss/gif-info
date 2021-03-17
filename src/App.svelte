<script>
  import { onMount } from "svelte";
  import FilePond, { registerPlugin } from "svelte-filepond";
  import FilePondPluginFileValidateType from "filepond-plugin-file-validate-type";
  import { parseGIF, decompressFrames } from "gifuct-js";

  import Frame from "./Frame.svelte";
  import Info from "./Info.svelte";
  import ColorTable from "./ColorTable.svelte";
  import Paster from "./Paster.svelte";

  registerPlugin(FilePondPluginFileValidateType);

  let pond;

  let gif;
  let frames;
  let decopressedFames;

  function handleAddFile(err, item) {
    if (typeof item.source === "string") {
      const search = new URLSearchParams();

      search.set("src", item.source);

      console.log(search);
      window.history.pushState(
        "object or string",
        "Title",
        `${location.origin}${location.pathname}?${search}`
      );
    }

    item.file
      .arrayBuffer()
      .then((buff) => {
        gif = parseGIF(buff);
        frames = gif.frames.filter((frame) => !!frame.image);
        return gif;
      })
      .then((gif) => {
        decopressedFames = decompressFrames(gif, true);
        console.log(gif, decopressedFames);
      });
  }

  onMount(() => {
    console.log(location.search.slice(1));
    const url = new URLSearchParams(location.search.slice(1));

    const src = url.get("src");
    if (src) {
      pond.addFile(src);
    }
  });
</script>

<main>
  <h1>Gif Info</h1>

  {#if decopressedFames}
    {#each decopressedFames as frame, index}
      <div>
        <h2>Frame {index + 1}</h2>
        <Frame
          frame={frame.patch}
          dims={frame.dims}
          width={gif.lsd.width}
          height={gif.lsd.height}
        />
        <Info
          dims={frame.dims}
          disposalType={frame.disposalType}
          delay={frame.delay}
        />
        <ColorTable
          exists={frames[index].image.descriptor.lct.exists}
          colors={frame.colorTable}
        />
      </div>
    {/each}
  {:else}
    <FilePond
      bind:this={pond}
      labelIdle="Drag & Drop your gif or <span class='filepond--label-action'> Browse </span> or Paste"
      onaddfile={handleAddFile}
      acceptedFileTypes="image/gif"
    />

    <Paster
      on:xpaste={(e) => {
        pond.addFile(e.detail.file);
      }}
    />
  {/if}
</main>

<style>
  main {
    height: 100%;
    max-width: 650px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
  }

  h1 {
    font-size: 4em;
    margin: 0;
  }

  h2 {
    margin: 0;
    padding: 32px 0 8px;
  }

  :global(.filepond--wrapper) {
    display: grid;
    align-items: center;
    flex-grow: 1;
  }
</style>
