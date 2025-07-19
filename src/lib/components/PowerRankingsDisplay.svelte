<script>
  export let nflState;
  export let rostersData;
  export let leagueTeamManagers;
  export let leagueData;
  export let playersInfo;

  // Sample power ranking logic: sort by total points descending
  let rankings = [];

  $: if (rostersData && leagueTeamManagers) {
    rankings = rostersData
      .map((roster) => {
        const manager = leagueTeamManagers.find((m) => m.roster_id === roster.roster_id);
        return {
          teamName: manager?.name ?? 'Unknown',
          avatar: manager?.avatar,
          record: roster.settings?.wins + '-' + roster.settings?.losses,
          points: roster.settings?.fpts ?? 0,
        };
      })
      .sort((a, b) => b.points - a.points)
      .map((entry, i) => ({ ...entry, rank: i + 1 }));
  }
</script>

<style>
  .ranking-list {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    padding-top: 0.25rem;
  }

  .rank-item {
    background-color: #fff;
    border-radius: 0.8rem;
    padding: 0.75rem 1rem;
    box-shadow: 0 1px 6px rgba(0, 0, 0, 0.1);
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 0.9rem;
  }

  .rank-number {
    font-weight: bold;
    width: 28px;
    color: #920505;
    text-align: center;
  }

  .team-info {
    flex-grow: 1;
    margin-left: 0.8rem;
  }

  .team-name {
    font-weight: 600;
  }

  .record {
    font-size: 0.7rem;
    color: #666;
  }

  .avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid #ddd;
  }

  .points {
    font-weight: 500;
    font-size: 0.8rem;
    color: #444;
    text-align: right;
  }
</style>

<div class="ranking-list">
  {#each rankings as team}
    <div class="rank-item">
      <div class="rank-number">#{team.rank}</div>
      <img class="avatar" src={team.avatar} alt={team.teamName} />
      <div class="team-info">
        <div class="team-name">{team.teamName}</div>
        <div class="record">{team.record}</div>
      </div>
      <div class="points">{team.points} pts</div>
    </div>
  {/each}
</div>
