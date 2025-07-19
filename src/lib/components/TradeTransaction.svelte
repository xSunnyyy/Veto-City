<script>
	import { gotoManager } from '$lib/utils/helper';
	import { getTeamFromTeamManagers } from '$lib/utils/helperFunctions/universalFunctions';
	import TransactionMove from './TransactionMove.svelte';

	export let transaction, players, leagueTeamManagers;
</script>

<style>
	.tradeTransaction {
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
		text-align: center;
		margin-bottom: 0.6rem;
	}

	.avatar {
		border-radius: 50%;
		height: 36px;
		width: 36px;
		border: 2px solid var(--blueTwo);
		background-color: #fff;
	}

	.ownerName {
		display: inline-block;
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

	table {
		width: 100%;
		border-collapse: collapse;
	}

	thead {
		margin-bottom: 0.5rem;
	}

	tbody {
		border-top: 1px solid #eee;
	}

	.holder {
		display: flex;
		flex-direction: column;
		align-items: center;
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
		.tradeTransaction {
			max-width: 92vw;
			padding: 0.7rem;
		}

		.ownerName {
			font-size: 0.8rem;
		}
	}
</style>

<div class="tradeTransaction">
	<table>
		<thead>
			<tr>
				{#each transaction.rosters as owner}
					<th class="name clickable" onclick={() => gotoManager({ year: transaction.season, leagueTeamManagers, rosterID: owner })}>
						<div class="holder">
							<img class="avatar" src="{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).avatar}" alt="{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name} avatar" />
							<span class="ownerName">
								{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name}
								{#if getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name !== getTeamFromTeamManagers(leagueTeamManagers, owner).name}
									<br />
									<span class="currentOwner">({getTeamFromTeamManagers(leagueTeamManagers, owner).name})</span>
								{/if}
							</span>
						</div>
					</th>
				{/each}
			</tr>
		</thead>
		<tbody>
			{#each transaction.moves as move}
				<TransactionMove {players} {move} type={transaction.type} {leagueTeamManagers} season={transaction.season} />
			{/each}
		</tbody>
	</table>
	<span class="date">{transaction.date}</span>
</div>
