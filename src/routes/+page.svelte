<script>
  import { PowerRankings, HomePost, Transactions } from '$lib/components';
  // Add more imports as needed for additional cards/sections
</script>

<style>
  .hero {
    background: linear-gradient(120deg, #920505 0%, #1a1a1a 100%);
    color: #fff;
    padding: 4rem 2rem 3rem 2rem;
    text-align: center;
    border-radius: 2rem;
    margin-bottom: 2.5rem;
    box-shadow: 0 8px 32px rgba(0,0,0,0.15);
  }
  .hero-title {
    font-size: 2.8rem;
    font-weight: 900;
    margin-bottom: 1rem;
    letter-spacing: 1px;
  }
  .hero-subtitle {
    font-size: 1.25rem;
    font-weight: 400;
    margin-bottom: 2rem;
    opacity: 0.93;
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
    box-shadow: 0 2px 8px rgba(146,5,5,0.09);
    transition: background 0.2s, color 0.2s;
  }
  .cta-btn:hover {
    background: #920505;
    color: #fff;
  }

  .sections {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    margin-bottom: 2.5rem;
  }

  .card {
    background: #fff;
    border-radius: 1.2rem;
    box-shadow: 0 2px 12px rgba(0,0,0,0.07);
    padding: 2rem 1.5rem;
    transition: box-shadow 0.2s;
    min-height: 250px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }
  .card:hover {
    box-shadow: 0 8px 24px rgba(146,5,5,0.18);
  }

  @media (max-width: 900px) {
    .sections {
      grid-template-columns: 1fr;
    }
  }
</style>

<div class="hero">
  <div class="hero-title">Welcome to Veto City</div>
  <div class="hero-subtitle">
  </div>
  <a href="/drafts"><button class="cta-btn">View League Drafts</button></a>
</div>

  <div class="card">
    <h2>Power Rankings</h2>
    <PowerRankings />
  </div>
  <div class="card">
    <h2>Recent Transactions</h2>
    <Transactions />
  </div>
  <!-- Add more cards for Standings, Rosters, Awards, etc. as needed -->
</div>
