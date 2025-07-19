<script>
  import { getNflState, getLeagueRosters, getLeagueTeamManagers, waitForAll, loadPlayers, getLeagueData } from '$lib/utils/helper';
  import PowerRankingsDisplay from './PowerRankingsDisplay.svelte';
  import LinearProgress from '@smui/linear-progress';

  let loading = true;
  let nflState, rostersData, leagueTeamManagers, leagueData, playersInfo;
  let error = null;

  onMount(async () => {
    try {
      [nflState, rostersData, leagueTeamManagers, leagueData, playersInfo] = await waitForAll(
        getNflState(),
        getLeagueRosters(),
        getLeagueTeamManagers(),
        getLeagueData(),
        loadPlayers(null)
      );
    } catch (err) {
      error = err;
    } finally {
      loading = false;
    }
  });
</script>

<style>
  .loading {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    text-align: center;
    color: var(--text-color, #333);
  }

  .error {
    color: red;
    padding: 1rem;
    text-align: center;
  }
</style>

{#if loading}
  <div class="loading">
    <p>Calculating power rankings...</p>
    <LinearProgress indeterminate />
  </div>
{:else if error}
  <div class="error">
    <p>Something went wrong: {error.message}</p>
  </div>
{:else if leagueData.status !== 'pre_draft' && leagueData.status !== 'complete'}
  <PowerRankingsDisplay
    {nflState}
    {rostersData}
    {leagueTeamManagers}
    {leagueData}
    {playersInfo}
  />
{:else}
  <p style="text-align:center;">Power rankings not available for this league state.</p>
{/if}
