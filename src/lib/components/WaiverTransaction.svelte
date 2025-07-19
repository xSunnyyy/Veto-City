<script>
	import { gotoManager } from '$lib/utils/helper';
	import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';

	export let transaction, players, leagueTeamManagers;

	const owner = transaction.rosters[0];

	const getAvatar = (pos, player) => {
		if (pos == 'DEF') {
			return `background-image: url(https://sleepercdn.com/images/team_logos/nfl/${player.toLowerCase()}.png)`;
		}
		return `background-image: url(https://sleepercdn.com/content/nfl/players/thumb/${player}.jpg), url(https://sleepercdn.com/images/v2/icons/player_default.webp)`;
	};
</script>

<style>
	.waiverTransaction {
		display: flex;
		flex-direction: column;
		width: 100%;
		max-width: 320px;
		margin: 1rem auto;
		padding: 1rem;
		border-radius: 1.2rem;
		background: rgba(255, 255, 255, 0.08);
		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);
		border: 1px solid rgba(255, 255, 255, 0.15);
		box-shadow: 0 4px 18px rgba(0, 0, 0, 0.2);
		color: #fff;
		overflow: hidden;
		transition: all 0.3s ease;
	}

	.name {
		position: relative;
		padding-left: 48px;
		margin-bottom: 0.5rem;
	}

	.avatar {
		border-radius: 50%;
		height: 40px;
		width: 40px;
		border: 2px solid var(--blueTwo);
		background-color: var(--fff);
		position: absolute;
		left: 0;
		top: 0;
	}

	.ownerName {
		display: inline-block;
		border-bottom: 2px solid var(--blueTwo);
		font-weight: 500;
		font-size: 0.9rem;
	}

	.currentOwner {
		font-style: italic;
		color: var(--aaa);
		font-size: 0.65rem;
		margin-left: 4px;
	}

	.core {
		border-top: 1px solid rgba(255, 255, 255, 0.1);
		padding-top: 0.5rem;
	}

	.avatarAndDetails {
		display: flex;
		flex-direction: column;
	}

	.details {
		display: flex;
		justify-content: center;
		flex-wrap: wrap;
		gap: 1.5rem;
		padding: 0.5rem 0.2rem;
	}

	.player {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
	}

	.playerAvatar {
		height: 50px;
		width: 50px;
		border-radius: 50%;
		background-position: center;
		background-size: auto 50px;
		background-repeat: no-repeat;
		border: 2px solid;
		position: relative;
	}

	.indicator {
		position: absolute;
		bottom: -8px;
		right: -8px;
		font-size: 18px;
	}

	.nameHolder {
		margin-top: 0.3rem;
	}

	.playerName {
		font-size: 0.8rem;
		font-weight: 500;
	}

	.playerInfo {
		font-size: 0.6rem;
		color: var(--g555);
	}

	.add {
		color: #00ceb8;
	}

	.drop {
		color: #ff2a6d;
	}

	.bid {
		color: var(--g555);
		font-style: italic;
		margin-left: 4px;
		font-size: 0.7rem;
	}

	.date {
		text-align: center;
		font-size: 0.7rem;
		color: var(--g999);
		font-style: italic;
		margin-top: 0.6rem;
	}

	.clickable {
		cursor: pointer;
	}

	@media (max-width: 420px) {
		.waiverTransaction {
			max-width: 90vw;
			padding: 0.8rem;
		}
	}
</style>

<div
	class="waiverTransaction clickable"
	on:click={() =>
		gotoManager({
			year: transaction.season,
			leagueTeamManagers,
			rosterID: owner
		})
	}
>
	<div class="name">
		<span class="ownerName">
			{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name}
			{#if getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name != getTeamFromTeamManagers(leagueTeamManagers, owner).name}
				<span class="currentOwner">({getTeamFromTeamManagers(leagueTeamManagers, owner).name})</span>
			{/if}
			{#if transaction.moves[0][0].bid}
				<span class="bid">- {transaction.moves[0][0].bid}$</span>
			{/if}
		</span>
		<img
			class="avatar"
			src="{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).avatar}"
			alt="{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name} avatar"
		/>
	</div>

	<div class="core">
		<div class="avatarAndDetails">
			<div class="details">
				<!-- Added Players -->
				{#each transaction.moves.filter(m => m[0].type === 'Added') as move}
					<div class="player">
						<div
							class="playerAvatar"
							style="border-color: var(--{players[move[0].player].pos}); background-color: var(--waiverAdd); {getAvatar(players[move[0].player].pos, move[0].player)}"
						>
							<i class="add indicator material-icons" aria-hidden="true">add_circle</i>
						</div>
						<span class="nameHolder">
							<span class="playerName">
								{players[move[0].player].fn} {players[move[0].player].ln}
							</span>
							<span class="playerInfo">
								{players[move[0].player].pos}
								{#if players[move[0].player].t} - {players[move[0].player].t}{/if}
							</span>
						</span>
					</div>
				{/each}

				<!-- Dropped Players -->
				{#each transaction.moves.filter(m => m[0].type === 'Dropped') as move}
					<div class="player">
						<div
							class="playerAvatar"
							style="border-color: var(--{players[move[0].player].pos}); background-color: var(--waiverDrop); {getAvatar(players[move[0].player].pos, move[0].player)}"
						>
							<i class="drop indicator material-icons" aria-hidden="true">do_not_disturb_on</i>
						</div>
						<span class="nameHolder">
							<span class="playerName">
								{players[move[0].player].fn} {players[move[0].player].ln}
							</span>
							<span class="playerInfo">
								{players[move[0].player].pos}
								{#if players[move[0].player].t} - {players[move[0].player].t}{/if}
							</span>
						</span>
					</div>
				{/each}
			</div>
		</div>
		<span class="date">{transaction.date}</span>
	</div>
</div>
