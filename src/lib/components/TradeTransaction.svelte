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
		max-width: 320px;
		margin: 1rem auto;
		padding: 1rem;
		border-radius: 1rem;
		background: #f8f8f8;
		border: 1px solid #ddd;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
		color: #000;
		overflow: hidden;
	}

	.name {
		text-align: center;
		font-size: 0.85rem;
		font-weight: 600;
		margin-bottom: 0.5rem;
	}

	.avatar {
		border-radius: 50%;
		height: 36px;
		width: 36px;
		border: 2px solid var(--blueOne);
		background-color: #fff;
	}

	.ownerName {
		display: inline-block;
		font-weight: 500;
		font-size: 0.85rem;
		margin: 0.2em;
		border-bottom: 2px solid var(--blueOne);
	}

	.currentOwner {
		font-style: italic;
		color: #888;
		font-size: 0.65rem;
	}

	.clickable {
		cursor: pointer;
	}

	.date {
		color: #777;
		font-style: italic;
		font-size: 0.65rem;
		text-align: center;
		margin-top: 0.6rem;
	}

	table {
		width: 100%;
		border-collapse: collapse;
		table-layout: fixed;
	}

	tbody {
		border-top: 1px solid #ddd;
	}

	.holder {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		gap: 0.25rem;
	}

	th.name {
		font-weight: 600;
		padding-bottom: 0.5rem;
	}

	@media (max-width: 420px) {
		.ownerName {
			font-size: 0.8rem;
		}

		.tradeTransaction {
			max-width: 92vw;
			padding: 0.8rem;
		}
	}
</style>

<div class="tradeTransaction">
	<table>
		<thead>
			<tr>
				{#each transaction.rosters as owner}
					<th
						class="name clickable"
						style="width: {1 / transaction.rosters.length * 100}%"
						on:click={() =>
							gotoManager({
								year: transaction.season,
								leagueTeamManagers,
								rosterID: owner
							})
						}
					>
						<div class="holder">
							<img
								class="avatar"
								src="{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).avatar}"
								alt="{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name} avatar"
							/>
							<span class="ownerName">
								{getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name}
								{#if getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name !== getTeamFromTeamManagers(leagueTeamManagers, owner).name}
									<br />
									<span class="currentOwner">
										({getTeamFromTeamManagers(leagueTeamManagers, owner).name})
									</span>
								{/if}
							</span>
						</div>
					</th>
				{/each}
			</tr>
		</thead>
		<tbody>
			{#each transaction.moves as move}
				<TransactionMove
					{players}
					{move}
					type={transaction.type}
					{leagueTeamManagers}
					season={transaction.season}
				/>
			{/each}
		</tbody>
	</table>
	<span class="date">{transaction.date}</span>
</div>
