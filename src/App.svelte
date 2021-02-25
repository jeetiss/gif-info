<script>
  import FilePond, { registerPlugin } from 'svelte-filepond';
  import FilePondPluginFileValidateType from 'filepond-plugin-file-validate-type';
  import { parseGIF, decompressFrames } from 'gifuct-js'

  import Frame from './Frame.svelte'
  import Info from './Info.svelte'
  import ColorTable from './ColorTable.svelte'

  registerPlugin(FilePondPluginFileValidateType)

  let pond

  let gif
  let frames
  let decopressedFames

  function handleAddFile(err, item) {
    item.file.arrayBuffer()
      .then(buff => {
        gif = parseGIF(buff);
        frames = gif.frames.filter(frame => !!frame.image)
        return gif
      })
      .then(gif => {
        decopressedFames = decompressFrames(gif, true)
        console.log(gif, decopressedFames)
      });
  }
</script>

<main>
	<h1>Gif Info</h1>
  
  {#if decopressedFames}
    {#each decopressedFames as frame, index}
      <div>
        <h2>Frame {index + 1}</h2>
        <Frame frame={frame.patch} dims={frame.dims} width={gif.lsd.width} height={gif.lsd.height}></Frame>
        <Info dims={frame.dims} disposalType={frame.disposalType} delay={frame.delay} />
        <ColorTable exists={frames[index].image.descriptor.lct.exists} colors={frame.colorTable}/>
      </div>
    {/each}
  {:else}
    <FilePond
      bind:this={pond}
      labelIdle='Drag & Drop your gif or <span class="filepond--label-action"> Browse </span>'
      onaddfile={handleAddFile}
      acceptedFileTypes='image/gif'
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
