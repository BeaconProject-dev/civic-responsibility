<script>
	import { fade, scale } from 'svelte/transition';
	import { X } from '@lucide/svelte';

	let { open = $bindable(), closeOnOutsideClick = true } = $props();

	let backdrop;

	function handleBackdropClick(event) {
		if (closeOnOutsideClick && event.target === backdrop) {
			open = false;
		}
	}

	function handleBackdropKeydown(event) {
		if (closeOnOutsideClick && event.key === 'Escape' && open) {
			open = false;
		}
	}

	function handleClose() {
		open = false;
	}

	// Prevent body scroll when modal is open
	$effect(() => {
		if (open) {
			document.body.style.overflow = 'hidden';
		} else {
			document.body.style.overflow = '';
		}

		return () => {
			document.body.style.overflow = '';
		};
	});
</script>

{#if open}
	<div
		class="modal-backdrop"
		transition:fade={{ duration: 200 }}
		bind:this={backdrop}
		onclick={handleBackdropClick}
		onkeydown={handleBackdropKeydown}
		role="dialog"
		aria-modal="true"
		tabindex="-1"
	>
		<div class="modal-container" transition:scale={{ duration: 200 }}>
			<button class="close-button" onclick={handleClose} aria-label="Close modal">
				<X size={24} />
			</button>
			<div class="modal-content">
				<slot />
			</div>
		</div>
	</div>
{/if}

<style lang="scss">
	@import '../styles/mixins.scss';

	.modal-backdrop {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.5);
		backdrop-filter: blur(4px);
		-webkit-backdrop-filter: blur(4px);
		display: flex;
        flex-direction: column;
		align-items: center;
		justify-content: center;
		z-index: 10000;
		padding: 1rem;
        max-height: 100dvh;

		@include mq('mobile', 'max') {
			padding: 0;
			align-items: flex-end;
		}
	}

	.modal-container {
		position: relative;
		border-radius: 16px;
		max-width: 900px;
		width: 100%;
		max-height: 90dvh;
		overflow-y: auto;
		box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
		display: flex;
		flex-direction: column;

		// @include mq('mobile', 'max') {
		// 	max-height: 95vh;
		// 	border-radius: 16px 16px 0 0;
		// 	max-width: 100%;
		// }

		.close-button {
			position: absolute;
			top: 1rem;
			right: 1rem;
			background: rgba(0, 0, 0, 0.05);
			border: none;
			border-radius: 50%;
			width: 40px;
			height: 40px;
			display: flex;
			align-items: center;
			justify-content: center;
			cursor: pointer;
			transition: all 0.2s ease;
			z-index: 10;
			color: var(--color-theme-dark);

			&:hover {
				background: rgba(0, 0, 0, 0.1);
				transform: scale(1.1);
			}

			&:active {
				transform: scale(0.95);
			}
		}

		.modal-content {
			padding: 0;
			overflow-y: auto;
		}
	}
</style>

