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
		margin: 1rem auto;
		width: 100%;
		max-width: 320px;
		border-radius: 1rem;
		padding: 0.5rem 0.8rem;
		background: rgba(255, 255, 255, 0.08);
		backdrop-filter: blur(10px);
		-webkit-backdrop-filter: blur(10px);
		border: 1px solid rgba(255, 255, 255, 0.15);
		box-shadow: 0 4px 18px rgba(0, 0, 0, 0.2);
		color: #fff;
		overflow: hidden;
	}

	.name {
		text-align: center;
		font-size: 0.85rem;
	}

	.avatar {
		border-radius: 50%;
		height: 36px;
		width: 36px;
		border: 2px solid var(--blueOne);
		background-color: var(--fff);
	}

	.ownerName {
		display: inline-block;
		font-weight: 500;
		line-height: 1.1em;
		margin: 0.2em;
		font-size: 0.82rem;
		text-align: center;
	}

	.currentOwner {
		font-style: italic;
		color: var(--aaa);
		font-size: 0.65rem;
	}

	.clickable {
		cursor: pointer;
	}

	.date {
		color: var(--g999);
		font-style: italic;
		font-size: 0.65rem;
		text-align: center;
		padding: 0.4em 0;
		margin-top: 1em;
		border-top: 1px solid rgba(255, 255, 255, 0.1);
	}

	table {
		width: 100%;
		border-collapse: collapse;
		table-layout: fixed;
	}

	tbody {
		border-top: 1px solid rgba(255, 255, 255, 0.12);
	}

	th {
		padding: 0.3rem 0.2rem;
	}

	.holder {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
	}

	@media (max-width: 420px) {
		.tradeTransaction {
			max-width: 90vw;
			padding: 0.5rem;
		}
		.ownerName {
			font-size: 0.75rem;
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
						onclick={() =>
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
								{#if
									getTeamFromTeamManagers(leagueTeamManagers, owner, transaction.season).name !=
									getTeamFromTeamManagers(leagueTeamManagers, owner).name
								}
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
