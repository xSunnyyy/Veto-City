<script>
  export let move, players, type, leagueTeamManagers, season;

  const getAvatar = (pos, playerId) => {
    if (pos === 'DEF') {
      return `background-image: url(https://sleepercdn.com/images/team_logos/nfl/${playerId.toLowerCase()}.png)`;
    }
    return `background-image: url(https://sleepercdn.com/content/nfl/players/thumb/${playerId}.jpg), url(https://sleepercdn.com/images/v2/icons/player_default.webp)`;
  };

  const teamName = (id) =>
    leagueTeamManagers.find(
      (m) => m.rosterID === id && (!season || m.season === season)
    )?.name || '';
</script>

<style>
  .moveRow {
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding: 0.6rem 0;
    border-top: 1px solid #ddd;
  }

  .playerCard {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 0.75rem;
    text-align: center;
    width: 48%;
  }

  .avatar {
    height: 48px;
    width: 48px;
    border-radius: 50%;
    background-position: center;
    background-size: auto 48px;
    background-repeat: no-repeat;
    border: 2px solid #ccc;
    margin-bottom: 0.3rem;
  }

  .name {
    font-weight: 600;
    font-size: 0.8rem;
  }

  .position {
    font-size: 0.65rem;
    color: #555;
  }

  .arrow {
    font-size: 1.5rem;
    color: #888;
    padding: 0 0.5rem;
  }

  @media (max-width: 420px) {
    .avatar {
      height: 42px;
      width: 42px;
      background-size: auto 42px;
    }

    .moveRow {
      flex-direction: column;
      gap: 0.4rem;
    }

    .arrow {
      display: none;
    }

    .playerCard {
      width: 100%;
    }
  }
</style>

<tr class="moveRow">
  {#if move[0] && players[move[0].player]}
    <td class="playerCard">
      <div
        class="avatar"
        style="{getAvatar(players[move[0].player].pos, move[0].player)}"
      ></div>
      <div class="name">
        {players[move[0].player].fn} {players[move[0].player].ln}
      </div>
      <div class="position">
        {players[move[0].player].pos}
        {#if players[move[0].player].t}
          - {players[move[0].player].t}
        {/if}
      </div>
    </td>
  {/if}

  <td class="arrow">⇄</td>

  {#if move[1] && players[move[1].player]}
    <td class="playerCard">
      <div
        class="avatar"
        style="{getAvatar(players[move[1].player].pos, move[1].player)}"
      ></div>
      <div class="name">
        {players[move[1].player].fn} {players[move[1].player].ln}
      </div>
      <div class="position">
        {players[move[1].player].pos}
        {#if players[move[1].player].t}
          - {players[move[1].player].t}
        {/if}
      </div>
    </td>
  {/if}
</tr>
