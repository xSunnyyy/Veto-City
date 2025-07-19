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
    <h6><center>Waivers</center></h6>
    {#each transactions.waivers as transaction}
      <WaiverTransaction {players} {transaction} {leagueTeamManagers} />
    {/each}
  {:else}
    <p>No waiver moves have been made yet...</p>
  {/if}
{/if}
