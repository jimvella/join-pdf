<script lang="ts">
  import { PDFDocument } from "pdf-lib";

  let files = $state();
  let files2 = $state();

  $effect(() => {
    if (files) {
      console.log("file", files[0]);
    }
  });

  const downloadURL = function (data, fileName) {
    let a;
    a = document.createElement("a");
    a.href = data;
    a.download = fileName;
    document.body.appendChild(a);
    a.style = "display: none";
    a.click();
    a.remove();
  };

  // Convert a binary large object | array into a downloadable data url
  // and trigger a download of it
  const downloadBlob = function (data, fileName, mimeType) {
    let blob, url;
    blob = new Blob([data], {
      type: mimeType,
    });
    url = window.URL.createObjectURL(blob);
    downloadURL(url, fileName);
    setTimeout(function () {
      return window.URL.revokeObjectURL(url);
    }, 1000);
  };

  const fileToPdf = async (file: File) => {
    const buffer = await file.arrayBuffer();
    return await PDFDocument.load(buffer);
  };
</script>

<h1>PDF Batch Prepend</h1>
<p>
  Prepend the first selected document to each of the second selected documents.
  Download using the same filenames as the second selected documents.
</p>
<div>
  <input type="file" accept="application/pdf" bind:files />
</div>
<div style="margin-top: 1em;">
  <input type="file" accept="application/pdf" bind:files={files2} multiple />
</div>
<div style="margin-top: 1em;">
  <button
    onclick={async () => {
      const prependPdf = await fileToPdf(files[0]);

      const prepend = async (file: File) => {
        const pdf = await fileToPdf(file);

        //prepend the first pdf onto the second
        const copiedPages = await pdf.copyPages(
          prependPdf,
          prependPdf.getPageIndices()
        );
        copiedPages.forEach((page, i) => {
          pdf.insertPage(i, page);
        });

        pdf.save().then((data) => {
          downloadBlob(data, file.name, file.type);
        });
      };

      console.log("files2", files2);

      [...files2].forEach((file) => {
        prepend(file);
      });
    }}>Merge</button
  >
</div>
