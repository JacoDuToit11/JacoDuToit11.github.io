---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /CV
---

{% include base_path %}

<style>
.cv-frame-wrap {
  width: 100%;
  height: 85vh;
  min-height: 600px;
  border: 1px solid #e1e1e1;
  border-radius: 6px;
  box-shadow: 0 2px 14px rgba(0, 0, 0, 0.08);
  overflow-y: auto;
  background: #525659;
  padding: 1em 0;
  text-align: center;
}
.cv-frame-wrap canvas {
  display: block;
  margin: 0 auto 1em;
  max-width: 95%;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.4);
  background: #fff;
}
.cv-status {
  color: #ddd;
  font-size: 0.95em;
  padding: 2em;
}
.cv-status a { color: #9ec5ff; }
</style>

<div class="cv-frame-wrap" id="cv-viewer">
  <p class="cv-status" id="cv-status">Loading CV&hellip;</p>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script>
(function () {
  var fileUrl = "{{ base_path }}/files/JacoDuToit_CV.pdf?v=2";
  var viewer = document.getElementById("cv-viewer");
  var status = document.getElementById("cv-status");

  function fail() {
    status.innerHTML =
      'Unable to display the CV inline. ' +
      '<a href="' + fileUrl + '">Open the PDF</a> instead.';
  }

  if (!window.pdfjsLib) { fail(); return; }
  pdfjsLib.GlobalWorkerOptions.workerSrc =
    "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

  pdfjsLib.getDocument(fileUrl).promise.then(async function (pdf) {
    if (status) status.remove();
    var dpr = window.devicePixelRatio || 1;
    for (var n = 1; n <= pdf.numPages; n++) {
      var page = await pdf.getPage(n);
      var targetWidth = viewer.clientWidth * 0.95;
      var base = page.getViewport({ scale: 1 });
      var scale = targetWidth / base.width;
      var viewport = page.getViewport({ scale: scale * dpr });
      var canvas = document.createElement("canvas");
      canvas.width = viewport.width;
      canvas.height = viewport.height;
      viewer.appendChild(canvas);
      await page.render({ canvasContext: canvas.getContext("2d"), viewport: viewport }).promise;
    }
  }).catch(fail);
})();
</script>
