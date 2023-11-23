<script>
  let isDark =
    document.querySelector("html").getAttribute("data-theme") == "dark";
  function toggleTheme() {
    let html = document.querySelector("html");
    if (html.getAttribute("data-theme") == "dark")
      html.setAttribute("data-theme", "light");
    else html.setAttribute("data-theme", "dark");
    isDark = !isDark;
  }
  import Icon from "@iconify/svelte";
</script>

<nav class="container-fluid">
  <ul><li><h3>Vitiligo Detector</h3></li></ul>
  <ul>
    <li>
      <a
        href="#"
        role="button"
        class="secondary"
        on:click={() => toggleTheme()}
      >
        {#if isDark}
          <Icon icon="ri:sun-fill" height="1.5rem" />
        {:else}
          <Icon icon="bxs:moon" height="1.5rem" />
        {/if}
      </a>
    </li>
  </ul>
</nav>

<style>
  h3 {
    margin-bottom: 0;
  }
  a {
    border-radius: 1.5rem;
  }
</style>
