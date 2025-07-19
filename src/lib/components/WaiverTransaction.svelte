<script>
	import { gotoManager } from '$lib/utils/helper';
	import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';

	export let transaction, players, leagueTeamManagers;

	const owner = transaction.rosters[0];

	const getAvatar = (pos, player) => {
		if (pos === 'DEF') {
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
		max-width: 300px;
		margin: 0.8rem auto;
		padding: 0.9rem;
		border-radius: 1rem;
		background: #f8f8f8;
		border: 1px solid #ddd;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
		color: #000;
		font-size: 0.85rem;
	}

	.name {
		position: relative;
		padding-left: 44px;
		margin-bottom: 0.5rem;
	}

	.avatar {
		position: absolute;
		left: 0;
		top: 0;
		width: 36px;
		height: 36px;
		border-radius: 50%;
		border: 2px solid var(--blueTwo);
		background-color: #fff;
	}

	.ownerName {
		font-weight: 600;
		font-size: 0.9rem;
		border-bottom: 2px solid var(--blueTwo);
	}

	.currentOwner {
		font-style: italic;
		color: #888;
		font-size: 0.65rem;
		margin-left: 4px;
	}

	.bid {
		color: #555;
		font-style: italic;
		margin-left: 4px;
		font-size: 0.7rem;
	}

	.core {
		border-top: 1px solid #eee;
		padding-top: 0.5rem;
	}

	.avatarAndDetails {
		display: flex;
		flex-direction: column;
	}

	.details {
		display: flex;
		justify-content: center;
		gap: 1.2rem;
		flex-wrap: wrap;
		padding: 0.4rem 0.2rem;
	}

	.player {
		display: flex;
		flex-direction: column;
		align-items: center;
		text-align: center;
	}

	.playerAvatar {
		width: 42px;
		height: 42px;
		border-radius: 50%;
		background-position: center;
		background-size: auto 42px;
		background-repeat: no-repeat;
		border: 2px solid;
		position: relative;
	}

	.indicator {
		position: absolute;
		bottom: -6px;
		right: -6px;
		font-size: 16px;
	}

	.add {
		color: #00ceb8;
	}

	.drop {
		color: #ff2a6d;
	}

	.nameHolder {
		margin-top: 0.25rem;
	}

	.playerName {
		font-weight: 500;
		font-size: 0.78rem;
	}

	.playerInfo {
		font-size: 0.62rem;
		color: #666;
	}

	.date {
		text-align: center;
		font-size: 0.65rem;
		color: #888;
		font-style: italic;
		margin-top: 0.5rem;
	}

	.clickable {
		cursor: pointer;
	}

	@media (max-width: 420px) {
		.waiverTransaction {
			max-width: 92vw;
			padding: 0.7rem;
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
			{#if getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name !== getTeamFromTeamManagers(leagueTeamManagers, owner).name}
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
