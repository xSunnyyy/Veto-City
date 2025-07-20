<style>
  :global(body) {
    margin: 0;
    padding: 0;
    background-color: #111; /* global black background */
    font-family: system-ui, sans-serif;
  }

  /* Only target these specific elements globally */
  :global(h1),
  :global(h5),
  :global(span),
  :global(input) {
    color: white !important;
  }

  .page-wrapper {
    background-color: #111;
    min-height: 100vh;
    padding: 2rem 1rem;
  }
</style>
