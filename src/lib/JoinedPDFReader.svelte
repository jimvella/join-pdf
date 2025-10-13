<script lang="ts">
  /*
        A view of multiple PDF documents such that 
        when the HTML page is printed the result is a 
        single concatenated PDF document. 
    */

  // @ts-nocheck
  import * as pdfjs from "pdfjs-dist";
  import * as pdfWorker from "pdfjs-dist/build/pdf.worker.mjs";
  import { onMount } from "svelte";
  import PDFPage from "./PDFPage.svelte";
  pdfjs.GlobalWorkerOptions.workerSrc =
    import.meta.url + "pdfjs-dist/build/pdf.worker.mjs;../$types.js";

  let { pdfPromise } = $props();
  let pages = $state([]);

  onMount(() => {
    pdfPromise.then((i) => {
      pages = i;
    });
  });
</script>

{#each pages as page}
  <div>
    <PDFPage {page} />
  </div>
{/each}
