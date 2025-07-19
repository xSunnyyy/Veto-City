<script>
  import { onMount } from 'svelte';
  import { getLeagueTransactions, getLeagueTeamManagers, loadPlayers, waitForAll } from '$lib/utils/helper';
  import TradeTransaction from './TradeTransaction.svelte';
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
  <p>Loading trades...</p>
  <LinearProgress indeterminate />
{:else}
  {#if transactions.trades.length}
    <h6><center>Trades</center></h6>
    {#each transactions.trades as transaction}
      <TradeTransaction {players} {transaction} {leagueTeamManagers} />
    {/each}
  {:else}
    <p>No trades have been made yet...</p>
  {/if}
{/if}
