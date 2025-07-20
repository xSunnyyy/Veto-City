<script>
    import {round} from '$lib/utils/helper';
	import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';

    export let matchup, players, active, ix, displayWeek, expandOverride=false, matchupWeek, leagueTeamManagers, year;

    let home = matchup[0];
    let away = matchup[1];

    let homePointsTotal = 0;
    let homeProjectionTotal = 0;
    let awayPointsTotal = 0;
    let awayProjectionTotal = 0;

    let winning = "home";

    const digestStarters = () => {
        home = matchup[0];
        away = matchup[1];
        home.manager = getTeamFromTeamManagers(leagueTeamManagers, home.roster_id, year);
        away.manager = getTeamFromTeamManagers(leagueTeamManagers, away.roster_id, year);
        const homeStarters = matchupWeek ? home.starters[matchupWeek] : home.starters;
        const awayStarters = matchupWeek ? away.starters[matchupWeek] : away.starters;
        const homePoints = matchupWeek ? home.points[matchupWeek] : home.points;
        const awayPoints = matchupWeek ? away.points[matchupWeek] : away.points;

        homePointsTotal = 0;
        homeProjectionTotal = 0;
        awayPointsTotal = 0;
        awayProjectionTotal = 0;

        const localStarters = [];
        for(let i = 0; i < homeStarters.length; i++) {
            homePointsTotal += homePoints[i];
            const awayPoint = awayPoints ? awayPoints[i] : 0;
            awayPointsTotal += awayPoint;
            const homePlayer = digestStarter(homeStarters[i], homePoints[i]);
            const awayStarter = awayStarters ? awayStarters[i] : null;
            const awayPlayer = digestStarter(awayStarter, awayPoint);
            homeProjectionTotal += homePlayer.projection;
            awayProjectionTotal += awayPlayer ? awayPlayer.projection : 0;
            localStarters.push({home: homePlayer, away: awayPlayer});
        }
        winning = awayPointsTotal < homePointsTotal ? "home" : awayPointsTotal > homePointsTotal ? "away" : "tied";
        starters = localStarters;
    }

    const digestStarter = (starter, points) => {
        if(!starter || starter === 0) {
            return {
                name: "Empty",
                avatar: null,
                pos: null,
                team: null,
                opponent: null,
                projection: 0,
                points: 0,
            };
        }
        const player = players[starter];
        let name = player.pos === "DEF" ? player.ln : `${player.fn[0]}. ${player.ln}`;
        let projection = player.wi?.[displayWeek]?.p ? parseFloat(player.wi[displayWeek].p) : 0;
        return {
            name,
            avatar: player.pos === "DEF"
                ? `background-image: url(https://sleepercdn.com/images/team_logos/nfl/${starter.toLowerCase()}.png)`
                : `background-image: url(https://sleepercdn.com/content/nfl/players/thumb/${starter}.jpg), url(https://sleepercdn.com/images/v2/icons/player_default.webp)`,
            pos: player.pos,
            team: player.t,
            opponent: player.wi?.[displayWeek]?.o ?? null,
            projection,
            points,
        };
    }

    let starters;

    $: digestStarters();

    let el;
    $: top = el?.getBoundingClientRect()?.top ?? 0;

    const expandClose = () => {
        if(expandOverride) return;
        active = active === ix ? null : ix;
        setTimeout(() => {
            window.scrollTo({left: 0, top, behavior: 'smooth'});
        }, 200);
    }

    let innerWidth;
    const calcHeight = () => {
        let multiplier = innerWidth < 410 ? 71 : innerWidth < 500 ? 72 : 73;
        const startersLength = matchupWeek ? home.starters[matchupWeek].length : home.starters.length;
        return startersLength * multiplier + 37;
    }
</script>

<svelte:window bind:innerWidth={innerWidth} />

<style>
    .matchup {
        width: 100%;
        max-width: 340px;
        margin: 1rem auto;
        font-size: 0.85rem;
        font-family: system-ui, sans-serif;
    }
    .header {
        display: flex;
        justify-content: space-between;
        border-radius: 1.2rem;
        background-color: #f3f3f3;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        padding: 0.75rem;
        cursor: pointer;
        transition: background 0.3s;
    }
    .header:hover {
        background-color: #e8e8e8;
    }
    .opponent {
        display: flex;
        align-items: center;
        gap: 0.5rem;
        width: 45%;
    }
    .avatar {
        border-radius: 50%;
        height: 30px;
        width: 30px;
    }
    .name {
        font-weight: 600;
        font-size: 0.9rem;
        color: #222;
    }
    .totalPoints {
        font-weight: 700;
        font-size: 0.9rem;
    }
    .rosters {
        max-height: 0;
        overflow: hidden;
        transition: max-height 0.4s ease;
        background-color: #fff;
        border-radius: 1rem;
        box-shadow: 0 1px 4px rgba(0,0,0,0.05);
        padding: 0 1rem;
    }
</style>

<div class="matchup">
  <div class="header" on:click={expandClose} bind:this={el}>
    <div class="opponent">
      <img class="avatar" src={home.manager.avatar} alt="home avatar" />
      <div class="name">{home.manager.name}</div>
    </div>
    <div class="totalPoints">{round(homePointsTotal)} - {round(awayPointsTotal)}</div>
    <div class="opponent">
      <div class="name">{away.manager.name}</div>
      <img class="avatar" src={away.manager.avatar} alt="away avatar" />
    </div>
  </div>
  <div
    class="rosters"
    style="max-height: {active === ix ? calcHeight() + 'px' : '0'}"
  >
    <!-- Optional expanded content -->
    {#if active === ix}
      {#each starters as player}
        <div>{player.home.name} vs {player.away.name}</div>
      {/each}
    {/if}
  </div>
</div>
