<script>
  import WaiverTransactions from '$lib/components/WaiverTransactions.svelte';
  import TradeTransactions from '$lib/components/TradeTransactions.svelte';
</script>

<style>
  :global(body) {
    margin: 0;
    padding: 0; /* simple dark background */
    font-family: system-ui, sans-serif;
  }

  .page-wrapper {
    min-height: 100vh;
    padding: 2rem 1rem;
  }

  .hero {
    color: #000;
    padding: 4rem 2rem 3rem 2rem;
    text-align: center;
    border-radius: 2rem;
    margin-bottom: 2.5rem;
  }

  .hero-title {
    font-size: 2.8rem;
    font-weight: 900;
    margin-bottom: 2rem;
    letter-spacing: 1px;
  }

  .cta-btn {
    background: #fff;
    color: #920505;
    border: none;
    border-radius: 2rem;
    padding: 0.75rem 2.5rem;
    font-size: 1.1rem;
    font-weight: 700;
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(146, 5, 5, 0.09);
  }

  .cta-btn:hover {
    background: #920505;
    color: #fff;
  }

  .sections {
    display: flex;
    justify-content: center;
    align-items: stretch;
    gap: 2rem;
    margin-bottom: 2.5rem;
    width: 100%;
    flex-wrap: wrap;
  }

  .card {
    background-color: #f3f3f3;
    color: #000;
    border-radius: 1.2rem;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    padding: 1.2rem 1rem;
    width: 100%;
    max-width: 340px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: stretch;
    font-size: 0.94rem;
    flex: 1 1 320px;
  }

  .card h2 {
    font-size: 1.05rem;
    margin-bottom: 0.6rem;
    text-align: center;
    width: 100%;
  }

  .card-content {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  @media (max-width: 1100px) {
    .sections {
      flex-wrap: wrap;
      gap: 1.2rem;
    }
  }

  @media (max-width: 900px) {
    .sections {
      flex-direction: column;
      align-items: center;
      gap: 1.5rem;
    }

    .card {
      max-width: 95vw;
    }
  }
</style>

<div class="page-wrapper">
  <div class="hero">
    <div class="hero-title">Welcome to Veto City</div>
    <a href="/drafts"><button class="cta-btn">View League Drafts</button></a>
  </div>

  <div class="sections">
    <div class="card">
      <div class="card-content">
        <WaiverTransactions />
      </div>
    </div>
    <div class="card">
      <div class="card-content">
        <TradeTransactions />
      </div>
    </div>
  </div>
</div>
