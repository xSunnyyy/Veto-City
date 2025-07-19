<script>
  import { PowerRankings, Transactions } from '$lib/components';
  // You may need to adjust PowerRankings if it doesn't support a "carousel" out of the box

  // For the carousel, we'll use a simple implementation with local state.
  let currentStart = 0;
  const visibleCount = 3;

  // Dummy data - replace with your real Power Rankings data or props as needed:
  // let powerRankingsData = [...] 

  // Simulate fetching/receiving data (assuming PowerRankings is a list/array):
  // If PowerRankings is a component that renders all, you may need to create
  // a new CarouselPowerRankings component that accepts "start" and "count" props.
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
  <!-- 
    If PowerRankings is a Svelte component that renders all rankings and can't accept "slice" props,
    you will need to refactor it to accept `start` and `count` props, or extract the data and render manually.
    Otherwise, this is pseudocode for a carousel.
  -->
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
    <h2 style="font-size: 1.3rem; color: #920505; margin-bottom: 0.7rem;">Recent Transactions</h2>
    <Transactions />
  </div>
</div>
