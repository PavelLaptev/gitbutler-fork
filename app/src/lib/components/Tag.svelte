<script lang="ts" context="module">
	export type TagStyle = 'neutral' | 'ghost' | 'pop' | 'success' | 'error' | 'warning' | 'purple';
	export type TagKind = 'soft' | 'solid';
</script>

<script lang="ts">
	import Icon from '$lib/components/Icon.svelte';
	import { tooltip } from '$lib/utils/tooltip';
	import type iconsJson from '$lib/icons/icons.json';

	// Interaction props
	export let help = '';
	export let disabled = false;
	export let clickable = false;
	export let loading = false;
	// Layout props
	export let shrinkable = false;
	export let verticalOrientation = false;
	export let icon: keyof typeof iconsJson | undefined = undefined;
	export let reversedDirection = false;
	// Style props
	export let style: TagStyle = 'neutral';
	export let kind: TagKind = 'soft';
</script>

<div
	class="tag text-base-11 text-semibold {style} {kind}"
	class:disabled
	class:shrinkable
	class:reversedDirection
	class:verticalOrientation
	class:not-button={!clickable}
	role={clickable ? 'button' : undefined}
	use:tooltip={help}
	on:click
	on:mousedown
	on:contextmenu
>
	<span class="label" class:verticalLabel={verticalOrientation}>
		<slot />
	</span>
	{#if loading}
		<Icon name="spinner" />
	{:else if icon}
		<div class="icon" class:verticalIcon={verticalOrientation}>
			<Icon name={icon} spinnerRadius={3.5} />
		</div>
	{/if}
</div>

<style lang="postcss">
	/* BASE */
	.tag {
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
		height: var(--size-control-tag);
		padding: var(--size-2) var(--size-4);
		border-radius: var(--radius-m);
		transition: background-color var(--transition-fast);

		/* component variables */
		--soft-bg-ratio: transparent 80%;
		--soft-hover-ratio: transparent 75%;
	}
	.icon {
		flex-shrink: 0;
	}
	.label {
		white-space: nowrap;
		display: inline-block;
		padding: 0 var(--size-2);
	}

	/* STYLES */
	.neutral {
		/* kind */
		&.soft {
			color: var(--clr-scale-ntrl-30);
			background: oklch(from var(--clr-core-ntrl-60) l c h / 0.15);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-ntrl-30);
				background: oklch(from var(--clr-core-ntrl-50) l c h / 0.18);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-ntrl-30);
			}
		}
		&.solid {
			color: var(--clr-scale-ntrl-100);
			background: var(--clr-scale-ntrl-30);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				background: var(--clr-scale-ntrl-30);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-30);
				background: var(--clr-scale-ntrl-100);
			}
		}
	}

	.ghost {
		&.soft,
		&.solid {
			color: var(--clr-scale-ntrl-40);
			background: transparent;
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-ntrl-20);
				background: oklch(from var(--clr-core-ntrl-60) l c h / 0.15);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-ntrl-30);
			}
		}

		&.solid {
			border: 1px solid oklch(from var(--clr-scale-ntrl-0) l c h / 0.2);

			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-ntrl-30);
				background: oklch(from var(--clr-core-ntrl-60) l c h / 0.1);
			}
		}
	}

	.pop {
		&.soft {
			color: var(--clr-theme-pop-on-container);
			background: var(--clr-scale-pop-80);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-pop-10);
				background: oklch(from var(--clr-scale-pop-80) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-pop-20);
			}
		}
		&.solid {
			color: var(--clr-theme-pop-on-element);
			background: var(--clr-theme-pop-element);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				background: oklch(from var(--clr-theme-pop-element) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-theme-pop-element);
				background: var(--clr-core-ntrl-100);
			}
		}
	}

	.success {
		&.soft {
			color: var(--clr-theme-succ-on-container);
			background: var(--clr-scale-succ-80);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-succ-10);
				background: oklch(from var(--clr-scale-succ-80) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-succ-20);
			}
		}
		&.solid {
			color: var(--clr-theme-succ-on-element);
			background: var(--clr-theme-succ-element);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				background: oklch(from var(--clr-theme-succ-element) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-theme-succ-element);
				background: var(--clr-core-ntrl-100);
			}
		}
	}

	.error {
		&.soft {
			color: var(--clr-theme-err-on-container);
			background: var(--clr-scale-err-80);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-err-10);
				background: oklch(from var(--clr-scale-err-80) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-err-20);
			}
		}
		&.solid {
			color: var(--clr-theme-err-on-element);
			background: var(--clr-theme-err-element);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				background: oklch(from var(--clr-theme-err-element) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-theme-err-element);
				background: var(--clr-core-ntrl-100);
			}
		}
	}

	.warning {
		&.soft {
			color: var(--clr-theme-warn-on-container);
			background: var(--clr-scale-warn-80);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-warn-10);
				background: oklch(from var(--clr-scale-warn-80) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-warn-20);
			}
		}
		&.solid {
			color: var(--clr-theme-warn-on-element);
			background: var(--clr-theme-warn-element);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				background: oklch(from var(--clr-theme-warn-element) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-theme-warn-element);
				background: var(--clr-core-ntrl-100);
			}
		}
	}

	.purple {
		&.soft {
			color: var(--clr-theme-purp-on-container);
			background: var(--clr-scale-purp-80);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				color: var(--clr-scale-purp-10);
				background: oklch(from var(--clr-scale-purp-80) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-scale-ntrl-100);
				background: var(--clr-scale-purp-20);
			}
		}
		&.solid {
			color: var(--clr-theme-purp-on-element);
			background: var(--clr-theme-purp-element);
			/* if button */
			&:not(.not-button, &:disabled):hover {
				background: oklch(from var(--clr-theme-purp-element) var(--hover-state-ratio) c h);
			}

			& .badge {
				--label-color: var(--clr-theme-purp-element);
				background: var(--clr-core-ntrl-100);
			}
		}
	}

	/* modifiers */

	.not-button {
		cursor: default;
		user-select: none;
	}

	.disabled {
		cursor: default;
		pointer-events: none;
		/* opacity: 0.5; */

		&.neutral.solid,
		&.pop.solid,
		&.success.solid,
		&.error.solid,
		&.warning.solid,
		&.purple.solid {
			color: var(--clr-bg-on-muted);
			background: oklch(from var(--clr-scale-ntrl-60) l c h / 0.15);
		}

		&.neutral.soft,
		&.pop.soft,
		&.success.soft,
		&.error.soft,
		&.warning.soft,
		&.purple.soft {
			color: var(--clr-bg-on-muted);
			background: oklch(from var(--clr-scale-ntrl-60) l c h / 0.15);
		}

		&.ghost {
			color: var(--clr-bg-on-muted);
		}

		&.ghost.solid {
			border: 1px solid oklch(from var(--clr-scale-ntrl-0) l c h / 0.1);
		}
	}

	.reversedDirection {
		flex-direction: row-reverse;
	}

	.shrinkable {
		overflow: hidden;

		& span {
			overflow: hidden;
			text-overflow: ellipsis;
		}
	}

	.verticalOrientation {
		writing-mode: vertical-rl;
		height: max-content;
		width: var(--size-control-tag);
		padding: var(--size-4) var(--size-2);
		transform: rotate(180deg);
	}

	.verticalIcon {
		transform: rotate(90deg);
	}

	.verticalLabel {
		padding: var(--size-2) 0;
	}
</style>
