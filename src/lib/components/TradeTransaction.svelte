<script>
  import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';
  import TradeMove from './TradeMove.svelte';

  export let trade, players, leagueTeamManagers;
  const teamA = trade.rosters[0];
  const teamB = trade.rosters[1];
</script>

<style>
  .trade-card {
    background: #fdfdfd;
    border: 1px solid #eee;
    border-radius: 1.2rem;
    padding: 1rem;
    margin: 1.5rem auto;
    width: 100%;
    max-width: 640px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;
  }

  .team {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    text-align: center;
    flex: 1;
    justify-content: center;
    flex-direction: column;
  }

  .team img {
    height: 48px;
    width: 48px;
    border-radius: 50%;
    border: 2px solid #002244;
  }

  .team-name {
    font-weight: bold;
    font-size: 0.9rem;
    border-bottom: 2px solid #002244;
  }

  .moves {
    display: flex;
    justify-content: space-between;
    margin-top: 0.75rem;
    padding-top: 0.75rem;
    border-top: 1px solid #ddd;
  }

  .move-column {
    flex: 1;
  }

  .arrow {
    font-size: 1.3rem;
    color: #888;
    text-align: center;
    flex: 0 0 40px;
    line-height: 1.6;
  }

  .date {
    margin-top: 0.75rem;
    font-size: 0.75rem;
    font-style: italic;
    color: #777;
    text-align: center;
  }

  @media (max-width: 500px) {
    .trade-card {
      padding: 0.75rem;
    }

    .team-name {
      font-size: 0.8rem;
    }

    .arrow {
      font-size: 1rem;
    }
  }
</style>

<div class="trade-card">
  <div class="header">
    <div class="team">
      <img src={getTeamFromTeamManagers(leagueTeamManagers, teamA)?.avatar} alt="Team A" />
      <div class="team-name">{getTeamFromTeamManagers(leagueTeamManagers, teamA)?.name}</div>
    </div>
    <div class="team">
      <img src={getTeamFromTeamManagers(leagueTeamManagers, teamB)?.avatar} alt="Team B" />
      <div class="team-name">{getTeamFromTeamManagers(leagueTeamManagers, teamB)?.name}</div>
    </div>
  </div>

  <div class="moves">
    <div class="move-column">
      {#each trade.moves.filter(m => m[0]?.roster_id === teamA) as move}
        <TradeMove move={move} players={players} />
      {/each}
    </div>
    <div class="arrow">⇄</div>
    <div class="move-column">
      {#each trade.moves.filter(m => m[0]?.roster_id === teamB) as move}
        <TradeMove move={move} players={players} />
      {/each}
    </div>
  </div>

  <div class="date">{trade.date}</div>
</div>
