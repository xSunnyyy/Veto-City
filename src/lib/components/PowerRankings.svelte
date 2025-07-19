<script>
  import { getNflState, getLeagueRosters, getLeagueTeamManagers, waitForAll, loadPlayers, getLeagueData } from '$lib/utils/helper';
  import PowerRankingsDisplay from './PowerRankingsDisplay.svelte';
  import LinearProgress from '@smui/linear-progress';

  const helperPromises = waitForAll(
    getNflState(),
    getLeagueRosters(),
    getLeagueTeamManagers(),
    getLeagueData(),
    loadPlayers(null)
  );
</script>

{#await helperPromises}
  <div>
    <p>Calculating power rankings...</p>
    <LinearProgress indeterminate />
  </div>
{:then [nflState, rostersData, leagueTeamManagers, leagueData, playersInfo]}
  {#if leagueData.status !== 'pre_draft' && leagueData.status !== 'complete'}
    <h6><center>Power Rankings</center></h6>
    <PowerRankingsDisplay
      {nflState}
      {rostersData}
      {leagueTeamManagers}
      {leagueData}
      {playersInfo}
    />
  {:else}
    <p>Power rankings will be available during the season.</p>
  {/if}
{:catch error}
  <p>Error loading power rankings: {error.message}</p>
{/await}
