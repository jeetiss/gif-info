<script>
  import { onMount, createEventDispatcher } from "svelte";

  const fireEvent = createEventDispatcher();

  onMount(() => {
    document.addEventListener("paste", (event) => {
      let clipboardData = event.clipboardData || window.clipboardData;
      if (clipboardData.files.length) {
        fireEvent("xpaste", { file: clipboardData.files[0] });
      } else {
        fireEvent("xpaste", { file: clipboardData.getData("text") });
      }
    });
  });
</script>
