<script lang="ts">
	import { clickOutDialog } from '$/actions/click_outside_dialog';
	import { episode_share_status, player } from '$/state/player';
	import { Show } from '@prisma/client';
	import ShareActions from './ShareActions.svelte';
	let modal: HTMLDialogElement;
	export let show: Show;
	export let timestamp = true;

	async function close() {
		$episode_share_status = false;
	}

	$: if ($episode_share_status) {
		if (modal) {
			modal.showModal();
		}
	}

	$: if (!$episode_share_status) {
		if (modal) {
			modal.close();
		}
	}
</script>

<dialog
	bind:this={modal}
	class="zone"
	style:--bg="var(--bg-sheet)"
	style:--fg="var(--fg-sheet)"
	use:clickOutDialog
	on:click-outside={close}
	aria-labelledby="search-header"
>
	<h3>Share</h3>
	<section aria-label="Share Window" class="share-window">
		<button on:click={close} class="close">×</button>
		<ShareActions {timestamp} {show} />
	</section>
</dialog>

<style lang="postcss">
	.close {
		position: absolute;
		top: 10px;
		right: 10px;
	}

	dialog::backdrop {
		background: rgba(0, 0, 0, 0.8);
	}
</style>
