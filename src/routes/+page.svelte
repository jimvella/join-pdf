<script lang="ts">
  import JoinedPDFReader from "$lib/JoinedPDFReader.svelte";
  import * as pdfjs from "pdfjs-dist";
  import * as pdfWorker from "pdfjs-dist/build/pdf.worker.mjs";

  let files = $state();
  let files2 = $state();
  let files3 = $state();
  let files4 = $state();
  let pdfPromise = $state();

  //   $effect(() => {
  //     console.log("files", files);
  //     if (files) {
  //       console.log("file", files);
  //     }
  //   });
</script>

{#if pdfPromise}
  <JoinedPDFReader {pdfPromise} />
{:else}
  <div>
    <input type="file" accept="application/pdf" bind:files />
  </div>
  <div>
    <input type="file" accept="application/pdf" bind:files={files2} />
  </div>
  <div>
    <input type="file" accept="application/pdf" bind:files={files3} />
  </div>
  <div>
    <input type="file" accept="application/pdf" bind:files={files4} />
  </div>

  <div style="margin-top: 1em;">
    <button
      onclick={() => {
        const promises = [files, files2, files3, files4]
          .filter((f) => f)
          .map((f) => {
            return new Promise((resolve, reject) => {
              const reader = new FileReader();
              reader.onload = (event) => {
                const buff = event.target?.result;

                const loadingTask = pdfjs.getDocument(buff);
                return loadingTask.promise
                  .then(function (pdf) {
                    return Promise.all(
                      Array(pdf._pdfInfo.numPages)
                        .fill()
                        .map((item, index) => {
                          const pageNumber = index + 1;
                          return pdf.getPage(pageNumber);
                        })
                    );
                  })
                  .then((i) => {
                    resolve(i);
                  });
              };
              reader.readAsArrayBuffer(f[0]);
            });
          });

        pdfPromise = new Promise((resolve, reject) => {
          Promise.all(promises).then((i) => {
            resolve(
              i.reduce((acc, n) => {
                return [...acc, ...n];
              }, [])
            );
          });
        });
      }}>Load PDF</button
    >
  </div>
{/if}
