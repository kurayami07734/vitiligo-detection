<script>
  let image = null,
    filename;
  let showImage = false;
  let result;
  function setImage(e) {
    if (e.target) {
      filename = e.target.files[0];
      let reader = new FileReader();
      reader.readAsDataURL(filename);
      reader.onload = (e) => (image.src = e.target.result);
      showImage = true;
    }
  }
  async function predict() {
    let data = new FormData();
    data.append("file", filename);
    const res = await fetch(
      "https://vitiligo-detector-7x6qvvfqha-el.a.run.app/predict",
      {
        method: "POST",
        body: data,
      }
    );
    const json = await res.json();
    result = json.result;
    modal.showModal();
  }
  function reset() {
    showImage = false;
    image = null;
  }
</script>

<dialog id="modal">
  <article>
    <header>
      {#if result >= 0.5}
        Warning
      {/if}
      {#if result < 0.5}
        Safe
      {/if}
    </header>
    {#if result >= 0.5}
      Our model detects vitiligo, please consider getting it checked by a
      medical professional
    {/if}
    {#if result < 0.5}
      Our model detects normal, healthy skin
    {/if}
    <footer><button on:click={() => modal.close()}>Close</button></footer>
  </article>
</dialog>

<article>
  <header>Upload your image to get started!</header>
  <input type="file" name="file" id="file" on:change={setImage} />
  {#if showImage}
    <img alt="uploaded photo" bind:this={image} />
    <footer>
      <a href="#" role="button" on:click={() => predict()}>
        Check this image
      </a>
      <a href="#" role="button" class="secondary" on:click={() => reset()}>
        Reset image
      </a>
    </footer>
  {/if}
</article>
