<script>
  import { round } from '$lib/utils/helper';
  import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';

  export let matchup, players, active, ix, displayWeek, expandOverride = false, matchupWeek, leagueTeamManagers, year;

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
    for (let i = 0; i < homeStarters.length; i++) {
      homePointsTotal += homePoints[i];
      const awayPoint = awayPoints ? awayPoints[i] : 0;
      awayPointsTotal += awayPoint;
      const homePlayer = digestStarter(homeStarters[i], homePoints[i]);
      const awayStarter = awayStarters ? awayStarters[i] : null;
      const awayPlayer = digestStarter(awayStarter, awayPoint);
      homeProjectionTotal += homePlayer.projection;
      awayProjectionTotal += awayPlayer ? awayPlayer.projection : 0;
      localStarters.push({ home: homePlayer, away: awayPlayer });
    }
    winning = awayPointsTotal < homePointsTotal ? "home" : awayPointsTotal > homePointsTotal ? "away" : "tied";
    starters = localStarters;
  };

  const digestStarter = (starter, points) => {
    if (!starter || starter === 0) {
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
  };

  let starters;
  $: digestStarters();

  let el;
  $: top = el?.getBoundingClientRect()?.top ?? 0;

  const expandClose = () => {
    if (expandOverride) return;
    active = active === ix ? null : ix;
    setTimeout(() => {
      window.scrollTo({ left: 0, top, behavior: 'smooth' });
    }, 200);
  };

  let innerWidth;
  const calcHeight = () => {
    let multiplier = innerWidth < 410 ? 71 : innerWidth < 500 ? 72 : 73;
    const startersLength = matchupWeek ? home.starters[matchupWeek].length : home.starters.length;
    return startersLength * multiplier + 37;
  };
</script>

<svelte:window bind:innerWidth={innerWidth} />

<style>
  .matchup {
    width: 100%;
    max-width: 400px;
    margin: 1rem auto;
    font-size: 0.85rem;
    font-family: system-ui, sans-serif;
    background-color: #111;
    border-radius: 1.2rem;
    box-shadow: 0 2px 10px rgba(255, 255, 255, 0.05);
    color: #fff;
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 1.2rem;
    background-color: #1a1a1a;
    padding: 0.75rem 1rem;
    cursor: pointer;
    transition: background 0.3s;
  }

  .header:hover {
    background-color: #222;
  }

  .opponent {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    flex: 1;
    min-width: 0;
  }

  .avatar {
    border-radius: 50%;
    height: 32px;
    width: 32px;
    border: 2px solid #333;
    object-fit: cover;
    flex-shrink: 0;
  }

  .name {
    font-weight: 600;
    font-size: 0.9rem;
    color: #fff;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .score {
    font-weight: bold;
    font-size: 0.95rem;
    padding: 0 0.5rem;
    color: #ccc;
  }

  .rosters {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.4s ease;
    background-color: #181818;
    border-radius: 0 0 1rem 1rem;
    padding: 0 1rem;
  }

  .rosters.open {
    padding-top: 0.5rem;
  }

  .expanded-line {
    padding: 0.3rem 0;
    border-bottom: 1px solid #333;
    color: #bbb;
  }

  @media (max-width: 480px) {
    .name {
      font-size: 0.8rem;
    }

    .score {
      font-size: 0.85rem;
    }
  }
</style>

<div class="matchup">
  <div class="header" on:click={expandClose} bind:this={el}>
    <div class="opponent">
      <img class="avatar" src={home.manager.avatar} alt="home avatar" />
      <div class="name">{home.manager.name}</div>
    </div>
    <div class="score">{round(homePointsTotal)} - {round(awayPointsTotal)}</div>
    <div class="opponent" style="justify-content: flex-end;">
      <div class="name">{away.manager.name}</div>
      <img class="avatar" src={away.manager.avatar} alt="away avatar" />
    </div>
  </div>

  <div class="rosters {active === ix ? 'open' : ''}" style="max-height: {active === ix ? calcHeight() + 'px' : '0'}">
    {#if active === ix}
      {#each starters as player}
        <div class="expanded-line">{player.home.name} vs {player.away.name}</div>
      {/each}
    {/if}
  </div>
</div>
