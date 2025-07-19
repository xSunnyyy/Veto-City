<script>
    // Carousel props for slicing
    export let start = 0;
    export let count = null;

    import { getNflState, getLeagueRosters, getLeagueTeamManagers, waitForAll, loadPlayers, getLeagueData } from '$lib/utils/helper';
    import PowerRankingsDisplay from './PowerRankingsDisplay.svelte';
    import LinearProgress from '@smui/linear-progress';

    const helperPromises = waitForAll(
        getNflState(),
        getLeagueRosters(),
        getLeagueTeamManagers(),
        getLeagueData(),
        loadPlayers(null),
    );
</script>

<style>
    .loading {
        display: block;
        width: 85%;
        max-width: 500px;
        margin: 80px auto;
    }
</style>

{#await helperPromises}
    <div class="loading">
        <p>Calculating power rankings...</p>
        <LinearProgress indeterminate />
    </div>
{:then [nflState, rostersData, leagueTeamManagers, leagueData, playersInfo]}
    {#if leagueData.status != 'pre_draft' && leagueData.status != 'complete'}
        {#if count === null}
            <PowerRankingsDisplay
                {nflState}
                {rostersData}
                {leagueTeamManagers}
                {leagueData}
                {playersInfo}
            />
        {:else}
            <PowerRankingsDisplay
                {nflState}
                rostersData={rostersData.slice(start, start + count)}
                {leagueTeamManagers}
                {leagueData}
                {playersInfo}
            />
        {/if}
    {/if}
{:catch error}
    <p>Something went wrong: {error.message}</p>
{/await}
