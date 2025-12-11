<script>
	import { base } from '$app/paths';

	let {
		size = 'large', // 'large', 'medium', 'small'
		showSeparator = true,
		className = '',
		interactive = true,
		justBeacon = false
	} = $props();
</script>

{#if interactive}
	<a
		href="https://beaconproject.us/"
		target="_blank"
		rel="noopener noreferrer"
		class="lockup {className} size-{size} interactive"
	>
		{#if !justBeacon}
			<img
				class="logo mic-logo"
				src={base + '/assets/icons/moreincommon.png'}
				alt="More in Common logo"
			/>
			{#if showSeparator}
				<div class="logo-separator"></div>
			{/if}
		{/if}
		<img
			class="logo beacon-logo"
			src={base + '/assets/icons/beaconproject.png'}
			alt="Beacon Project logo"
		/>
	</a>
{:else}
	<div class="lockup {className} size-{size}">
		{#if !justBeacon}
			<img
				class="logo mic-logo"
				src={base + '/assets/icons/moreincommon.png'}
				alt="More in Common logo"
			/>

			{#if showSeparator}
				<div class="logo-separator"></div>
			{/if}
		{/if}
		<img
			class="logo beacon-logo"
			src={base + '/assets/icons/beaconproject.png'}
			alt="Beacon Project logo"
		/>
	</div>
{/if}

<style lang="scss">
	@import '../styles/mixins.scss';

	.lockup {
		display: flex;
		align-items: center;
		gap: $spacing-md;
		pointer-events: none;
		flex-shrink: 0; // Prevent lockup from squishing
		text-decoration: none;
		color: inherit;

		&:hover {
			opacity: 0.8;
		}
		&.interactive {
			pointer-events: auto;
		}
		@include mq('mobile', 'max') {
			gap: 0px;
		}

		.logo {
			width: auto;
			transition: opacity 0.3s ease;
			flex-shrink: 0; // Prevent logos from squishing
			display: block;
			line-height: 0;
		}

		.logo-separator {
			width: 2px;
			background-color: #fff;
		}

		// Size variants
		&.size-large {
			.logo {
				height: 80px;
			}

			.logo-separator {
				height: 60px;
			}
		}

		&.size-medium {
			.logo {
				height: 45px;
			}

			.logo-separator {
				height: 35px;
				width: 1px;
			}
		}

		&.size-small {
			.logo {
				height: 24px;
			}

			.logo-separator {
				height: 18px;
			}
		}
	}
</style>
