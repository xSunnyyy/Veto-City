<script>
  import { onMount } from 'svelte';
  import { getLeagueTransactions, getLeagueTeamManagers, loadPlayers, waitForAll } from '$lib/utils/helper';
  import WaiverTransaction from './WaiverTransaction.svelte';
  import LinearProgress from '@smui/linear-progress';

  let loading = true;
  let players, transactions, leagueTeamManagers;

  onMount(async () => {
    const [transactionsData, playersData, leagueTeamManagersData] = await waitForAll(
      getLeagueTransactions(true),
      loadPlayers(null),
      getLeagueTeamManagers()
    );
    players = playersData.players;
    transactions = transactionsData.transactions;
    leagueTeamManagers = leagueTeamManagersData;
    loading = false;
  });
</script>

{#if loading}
  <p>Loading waiver moves...</p>
  <LinearProgress indeterminate />
{:else}
  {#if transactions.waivers.length}
    <div class="waiver-wrapper">
      {#each transactions.waivers as transaction}
        <WaiverTransaction {players} {transaction} {leagueTeamManagers} />
      {/each}
    </div>
  {:else}
    <p>No waiver moves have been made yet...</p>
  {/if}
{/if}

<style>
  .waiver-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    padding: 0 1rem;
  }

  :global(.waiverTransaction) {
    max-width: 300px;
    width: 100%;
  }
</style>
