<script>
  export let type = null; // "trade", "waiver", or null for all

  // Example: Fetch or receive all transactions as a prop or from a helper
  // You may need to adjust this based on your actual data source
  import { onMount } from "svelte";
  import LinearProgress from '@smui/linear-progress';
  import { getTransactions } from "$lib/utils/helper"; // Adjust path as needed

  let transactions = [];
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      transactions = await getTransactions();
    } catch (e) {
      error = e;
    } finally {
      loading = false;
    }
  });

  // Filtering logic
  $: filteredTransactions = type
    ? transactions.filter(tx => tx.type === type)
    : transactions;
</script>

<style>
  .loading {
    display: block;
    width: 95%;
    max-width: 400px;
    margin: 20px auto;
  }
  ul {
    padding-left: 1.2em;
    margin: 0.4em 0;
    font-size: 1em;
  }
  li {
    margin-bottom: 0.4em;
    line-height: 1.4;
    border-bottom: 1px solid #eee;
    padding-bottom: 0.3em;
  }
  .empty {
    color: #888;
    text-align: left;
    font-size: 0.97em;
    margin: 0.8em 0 0.2em 0;
  }
</style>

{#if loading}
  <div class="loading">
    <LinearProgress indeterminate />
    <div style="margin-top:0.7em;">Loading transactions...</div>
  </div>
{:else if error}
  <div class="loading">
    <span style="color:#b00;">Error loading transactions: {error.message}</span>
  </div>
{:else if filteredTransactions.length === 0}
  <div class="empty">
    {#if type === "trade"}
      No recent trades.
    {:else if type === "waiver"}
      No recent waiver moves.
    {:else}
      No recent transactions.
    {/if}
  </div>
{:else}
  <ul>
    {#each filteredTransactions as tx (tx.id)}
      <li>
        {#if tx.type === "trade"}
          <b>Trade:</b>
        {:else if tx.type === "waiver"}
          <b>Waiver:</b>
        {:else}
          <b>{tx.type}:</b>
        {/if}
        <!-- Render transaction summary. Adjust based on your tx structure -->
        {tx.summary || tx.description || tx.details || JSON.stringify(tx)}
        <br />
        <small style="color:#888;">
          {tx.date ? new Date(tx.date).toLocaleString() : ""}
        </small>
      </li>
    {/each}
  </ul>
{/if}