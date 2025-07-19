<script>
  import { PowerRankings } from '$lib/components';
  import Transactions from '$lib/Transactions/Transactions.svelte';

  // Carousel state for PowerRankings
  let currentStart = 0;
  const visibleCount = 3;

  // --- Filtering logic for Transactions ---
  // Assume Transactions can accept a 'type' prop or similar to filter trades/waivers
  // If not, you'll need to implement filtering inside Transactions or make wrapper components

  // Example types: "trade", "waiver"
</script>

<style>
  .carousel-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 2rem;
    width: 100%;
    max-width: 900px;
    margin-left: auto;
    margin-right: auto;
  }
  .carousel-controls {
    display: flex;
    justify-content: center;
    margin-top: 0.5rem;
    gap: 1rem;
  }
  .carousel-btn {
    background: #920505;
    color: #fff;
    border: none;
    border-radius: 50%;
    width: 2.4rem;
    height: 2.4rem;
    font-size: 1.1rem;
    cursor: pointer;
    transition: background 0.2s;
  }
  .carousel-btn:disabled {
    background: #bbb;
    color: #fff;
    cursor: not-allowed;
  }
  .carousel-title {
    font-size: 2.2rem;
    font-weight: 800;
    text-align: center;
    margin-bottom: 0.8rem;
    color: #920505;
    letter-spacing: 1px;
  }

  .sections {
    display: flex;
    justify-content: center;
    gap: 2rem;
    margin-bottom: 2.5rem;
    flex-wrap: wrap;
  }

  .card-small {
    background: #fff;
    border-radius: 1.2rem;
    box-shadow: 0 2px 12px rgba(0,0,0,0.07);
    padding: 1rem 1.2rem;
    min-width: 320px;
    max-width: 440px;
    min-height: 80px;
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  @media (max-width: 900px) {
    .sections {
      flex-direction: column;
      align-items: center;
      gap: 1.5rem;
    }
    .carousel-container {
      max-width: 100%;
    }
  }
</style>

<div class="carousel-container">
  <div class="carousel-title">Power Rankings</div>
  <PowerRankings start={currentStart} count={visibleCount} />
  <div class="carousel-controls">
    <button class="carousel-btn" on:click={() => currentStart = Math.max(0, currentStart - 1)} disabled={currentStart === 0} aria-label="Previous">
      &#8592;
    </button>
    <button class="carousel-btn" on:click={() => currentStart = currentStart + 1} aria-label="Next">
      &#8594;
    </button>
  </div>
</div>

<div class="sections">
  <div class="card-small">
    <h2 style="font-size: 1.2rem; color: #920505; margin-bottom: 0.7rem;">Recent Trades</h2>
    <Transactions type="trade" />
  </div>
  <div class="card-small">
    <h2 style="font-size: 1.2rem; color: #920505; margin-bottom: 0.7rem;">Recent Waiver Moves</h2>
    <Transactions type="waiver" />
  </div>
</div>
