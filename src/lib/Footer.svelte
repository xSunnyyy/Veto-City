<script>
	import { goto } from '$app/navigation';
	import { managers } from '$lib/utils/helper';
	import { tabs } from '$lib/utils/tabs';
	import { onMount } from 'svelte';

	let outOfDate = false;
	let el, footerHeight;
	let innerWidth;

	const resize = (e, delay) => {
		const bottom = el?.getBoundingClientRect().bottom;
		const top = el?.getBoundingClientRect().top;
		if (delay) {
			setTimeout(() => {
				resize(e, false);
			}, 100);
		} else {
			footerHeight = bottom - top;
		}
	};

	onMount(async () => {
		const res = await fetch('/api/checkVersion', { compress: true });
		const needUpdate = await res.json();
		outOfDate = needUpdate;
		resize(el?.getBoundingClientRect(), true);
	});

	let managersOutOfDate = false;
	if (managers) {
		for (const manager of managers) {
			if (manager.roster && !manager.managerID) {
				managersOutOfDate = true;
				resize(el?.getBoundingClientRect(), true);
				break;
			}
		}
	}

	const year = new Date().getFullYear();
	$: resize(el?.getBoundingClientRect(), false, innerWidth);
</script>

<svelte:window bind:innerWidth={innerWidth} />

<style>
	footer {
		width: 100%;
		display: block;
		position: absolute;
		bottom: 0;
		z-index: 1;
		padding: 30px 0 60px;
		text-align: center;
		color: #ddd;
		border-top: 1px solid rgba(255, 255, 255, 0.1);

		/* Glassmorphism background */
		background: rgba(18, 18, 18, 0.4);
		backdrop-filter: blur(12px);
		-webkit-backdrop-filter: blur(12px);
		box-shadow: 0 -8px 32px rgba(0, 0, 0, 0.3);
	}

	#navigation {
		margin: 0 0 2em;
	}

	#navigation ul {
		margin: 0;
		padding: 0;
	}

	#navigation ul li {
		list-style-type: none;
		display: inline;
	}

	#navigation li:not(:first-child):before {
		content: " | ";
		color: #999;
	}

	.navLink {
		display: inline-block;
		cursor: pointer;
		padding: 6px 10px;
		color: #eee;
		transition: color 0.2s ease;
	}

	.navLink:hover {
		color: #ff4b4b;
	}

	.updateNotice {
		color: #aaa;
		font-style: italic;
		font-size: 0.8em;
		margin-top: 0;
	}
</style>

<div class="footerSpacer" style="height: {footerHeight}px;" />

<footer bind:this={el}>
	{#if outOfDate}
		<p class="updateNotice">
			There is an update available for your League Page.
			<a href="https://github.com/nmelhado/league-page/blob/master/TRAINING_WHEELS.md#iv-updates">Follow the Update Instructions</a> to get all of the newest features!
		</p>
	{/if}
	{#if managersOutOfDate}
		<p class="updateNotice">
			Your managers page needs an update,
			<a href="https://github.com/nmelhado/league-page/blob/master/TRAINING_WHEELS.md#2-add-managers">please follow the instructions</a> to get the most up-to-date experience.
		</p>
	{/if}
	<div id="navigation">
		<ul>
			{#each tabs as tab}
				{#if !tab.nest}
					<li><div class="navLink" on:click={() => goto(tab.dest)}>{tab.label}</div></li>
				{:else}
					{#each tab.children as child}
						{#if child.label != "Managers" || managers.length > 0}
							{#if child.label == "Go to Sleeper"}
								<li><div class="navLink" on:click={() => (window.location = child.dest)}>{child.label}</div></li>
							{:else}
								<li><div class="navLink" on:click={() => goto(child.dest)}>{child.label}</div></li>
							{/if}
						{/if}
					{/each}
				{/if}
			{/each}
		</ul>
	</div>
</footer>
