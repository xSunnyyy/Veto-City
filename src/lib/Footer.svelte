<script>
  import { PowerRankings } from '$lib/components';
  import WaiverTransactions from '$lib/components/WaiverTransactions.svelte';
  import TradeTransactions from '$lib/components/TradeTransactions.svelte';
  import { goto } from '$app/navigation';
  import { tabs } from '$lib/utils/tabs';
  import { managers } from '$lib/utils/helper';
  import { onMount } from 'svelte';

  let outOfDate = false;
  let footerHeight;
  let el;
  let innerWidth;

  const resize = (e, delay) => {
    const bottom = el?.getBoundingClientRect().bottom;
    const top = el?.getBoundingClientRect().top;
    if (delay) {
      setTimeout(() => resize(e, false), 100);
    } else {
      footerHeight = bottom - top;
    }
  };

  onMount(async () => {
    const res = await fetch('/api/checkVersion', { compress: true });
    const needUpdate = await res.json();
    outOfDate = needUpdate;
    resize(el?.getBoundingClientRect(), true);
  });

  let managersOutOfDate = false;
  if (managers) {
    for (const manager of managers) {
      if (manager.roster && !manager.managerID) {
        managersOutOfDate = true;
        resize(el?.getBoundingClientRect(), true);
        break;
      }
    }
  }

  $: resize(el?.getBoundingClientRect(), false, innerWidth);
</script>

<svelte:window bind:innerWidth={innerWidth} />

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    background-color: #111;
    font-family: system-ui, sans-serif;
  }

  .page-wrapper {
    background-color: #111;
    min-height: 100vh;
    padding: 2rem 1rem;
    padding-bottom: 6rem; /* Leave room for footer */
  }

  .hero {
    color: #fff;
    padding: 4rem 2rem 3rem 2rem;
    text-align: center;
    border-radius: 2rem;
    margin-bottom: 2.5rem;
    background-color: #1e1e1e;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
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

  footer {
    background-color: #1e1e1e;
    width: 100%;
    display: block;
    position: absolute;
    bottom: 0;
    z-index: 1;
    border-top: 1px solid #920505;
    padding: 30px 0 60px;
    text-align: center;
    color: #bbb;
  }

  #navigation {
    margin: 0 0 2em;
  }

  #navigation ul {
    margin: 0;
    padding: 0;
  }

  #navigation ul li {
    list-style-type: none;
    display: inline;
  }

  #navigation li:not(:first-child):before {
    content: " | ";
  }

  .navLink {
    display: inline-block;
    cursor: pointer;
    padding: 6px 10px;
    color: #bbb;
  }

  .navLink:hover {
    color: #fff;
  }

  .updateNotice {
    color: #888;
    font-style: italic;
    font-size: 0.8em;
    margin-top: 0;
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
        <h2>Power Rankings</h2>
        <PowerRankings />
      </div>
    </div>
    <div class="card">
      <div class="card-content">
        <h2>Recent Waiver Moves</h2>
        <WaiverTransactions />
      </div>
    </div>
    <div class="card">
      <div class="card-content">
        <h2>Recent Trades</h2>
        <TradeTransactions />
      </div>
    </div>
  </div>
</div>

<!-- Footer Spacer for layout -->
<div class="footerSpacer" style="height: {footerHeight}px;" />

<!-- Footer -->
<footer bind:this={el}>
  {#if outOfDate}
    <p class="updateNotice">There is an update available for your League Page. <a href="https://github.com/nmelhado/league-page/blob/master/TRAINING_WHEELS.md#iv-updates">Follow the Update Instructions</a> to get all of the newest features!</p>
  {/if}
  {#if managersOutOfDate}
    <p class="updateNotice">Your managers page needs an update, <a href="https://github.com/nmelhado/league-page/blob/master/TRAINING_WHEELS.md#2-add-managers">please follow the instructions</a> to get the most up-to-date experience.</p>
  {/if}
  <div id="navigation">
    <ul>
      {#each tabs as tab}
        {#if !tab.nest}
          <li><div class="navLink" on:click={() => goto(tab.dest)}>{tab.label}</div></li>
        {:else}
          {#each tab.children as child}
            {#if child.label !== "Managers" || managers.length > 0}
              {#if child.label === "Go to Sleeper"}
                <li><div class="navLink" on:click={() => (window.location = child.dest)}>{child.label}</div></li>
              {:else}
                <li><div class="navLink" on:click={() => goto(child.dest)}>{child.label}</div></li>
              {/if}
            {/if}
          {/each}
        {/if}
      {/each}
    </ul>
  </div>
</footer>
