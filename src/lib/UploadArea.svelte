<script>
  let image = null,
    filename;
  let showImage = false;
  let isLoading = false;
  let result;
  function setImage(e) {
    if (e.target) {
      filename = e.target.files[0];
      let reader = new FileReader();
      reader.readAsDataURL(filename);
      reader.onload = (e) => {
        image.src = e.target.result;
      };
      showImage = true;
    }
  }
  async function predict() {
    let data = new FormData();
    data.append("file", filename);
    isLoading = true;
    const res = await fetch(
      "https://vitiligo-detector-api.onrender.com/predict",
      {
        method: "POST",
        body: data,
      }
    );
    const json = await res.json();
    result = json.result;
    modal.showModal();
    isLoading = false;
  }
  function reset() {
    showImage = false;
    image = null;
    isLoading = false;
  }
</script>

<dialog id="modal">
  <article>
    <header>
      {#if result >= 0.5}
        Warning
      {:else if result < 0.5}
        Safe
      {/if}
    </header>
    {#if result >= 0.5}
      Our model detects vitiligo, please consider getting it checked by a
      medical professional
    {:else if result < 0.5}
      Our model detects normal, healthy skin
    {/if}
    <footer><button on:click={() => modal.close()}>Close</button></footer>
  </article>
</dialog>

<article>
  <header>Upload your image to get started!</header>
  <h3>Disclaimer: This model was created for educational purposes and is not a replacement of medical advice.</h3>
  <h2>For best results, crop the image to include only the region of interest</h2>
  <p>The model has been moved to free tier of render.com, hence it may take longer to return a result</p>
  <input
    type="file"
    name="file"
    id="upload"
    accept="image/jpeg, image/png"
    on:change={setImage}
  />
  {#if showImage}
    <img alt="uploaded photo" bind:this={image} />
    <footer>
      <a
        href="#"
        role="button"
        aria-busy={isLoading}
        on:click|preventDefault={() => predict()}
      >
        Check this image
      </a>
      <a href="#" role="button" class="secondary" on:click={() => reset()}>
        Reset image
      </a>
    </footer>
  {/if}
</article>

<style>
  img {
    max-width: min(250px, 60vw);
  }
</style>
