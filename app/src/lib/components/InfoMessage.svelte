<script lang="ts" context="module">
	export type MessageStyle = 'neutral' | 'error' | 'pop' | 'warn' | 'success';
</script>

<script lang="ts">
	import Button, { type ButtonStyle } from './Button.svelte';
	import Icon, { type IconColor } from '$lib/components/Icon.svelte';
	import { createEventDispatcher } from 'svelte';
	import type iconsJson from '../icons/icons.json';

	export let icon: keyof typeof iconsJson | undefined = undefined;
	export let style: MessageStyle = 'neutral';
	export let outlined: boolean = true;
	export let filled: boolean = false;
	export let title: string | undefined = undefined;
	export let primary: string | undefined = undefined;
	export let secondary: string | undefined = undefined;
	export let shadow = false;

	const SLOTS = $$props.$$slots;
	const dispatch = createEventDispatcher<{ primary: void; secondary: void }>();

	const iconMap: { [Key in MessageStyle]: keyof typeof iconsJson } = {
		neutral: 'info',
		pop: 'info',
		warn: 'warning',
		error: 'error',
		success: 'success'
	};

	const iconColorMap: { [Key in MessageStyle]: IconColor } = {
		neutral: 'pop',
		pop: 'pop',
		warn: 'warn',
		error: 'error',
		success: 'success'
	};

	const primaryButtonMap: { [Key in MessageStyle]: ButtonStyle } = {
		neutral: 'pop',
		pop: 'pop',
		warn: 'warning',
		error: 'error',
		success: 'pop'
	};
</script>

<div
	class="info-message"
	class:neutral={style == 'neutral'}
	class:error={style == 'error'}
	class:pop={style == 'pop'}
	class:warn={style == 'warn'}
	class:success={style == 'success'}
	class:has-border={outlined}
	class:has-background={filled}
	class:shadow
>
	<Icon name={icon ? icon : iconMap[style]} color={iconColorMap[style]} />
	<div class="info-message__inner">
		<div class="info-message__content">
			{#if title || SLOTS.title}
				<div class="info-message__title text-base-body-13 text-semibold">
					{#if title}
						{title}
					{:else}
						<slot name="title" />
					{/if}
				</div>
			{/if}
			{#if SLOTS.content}
				<slot name="content" />
			{:else}
				<div class="info-message__text text-base-body-12"><slot /></div>
			{/if}
		</div>
		{#if primary || secondary}
			<div class="info-message__actions">
				{#if secondary}
					<Button style="ghost" kind="solid" on:click={() => dispatch('secondary')}>
						{secondary}
					</Button>
				{/if}
				{#if primary}
					<Button style={primaryButtonMap[style]} kind="solid" on:click={() => dispatch('primary')}>
						{primary}
					</Button>
				{/if}
			</div>
		{/if}
	</div>
</div>

<style lang="postcss">
	.info-message {
		color: var(--clr-scale-ntrl-0);
		display: flex;
		padding: var(--size-16);
		border-radius: var(--radius-m);
		gap: var(--size-12);
		background-color: var(--clr-bg-main);
		transition:
			background-color var(--transition-slow),
			border-color var(--transition-slow);
	}
	.info-message__inner {
		display: flex;
		flex-grow: 1;
		flex-direction: column;
		gap: var(--size-12);
		overflow-x: hidden;
	}
	.info-message__content {
		display: flex;
		flex-direction: column;
		gap: var(--size-8);
		user-select: text;
	}
	.info-message__actions {
		display: flex;
		gap: var(--size-6);
		justify-content: flex-end;
	}
	.info-message__text {
		&:empty {
			display: none;
		}
	}

	/* MODIFIERS */
	.neutral {
		border: 0 solid var(--clr-border-main);
	}
	.error {
		border: 0 solid var(--clr-scale-err-60);
	}
	.pop {
		border: 0 solid var(--clr-scale-pop-50);
	}
	.warn {
		border: 0 solid var(--clr-scale-warn-60);
	}
	.success {
		border: 0 solid var(--clr-scale-succ-60);
	}
	.shadow {
		box-shadow: 0px 7px 14px 0px rgba(0, 0, 0, 0.1);
	}

	/* OUTLINED */

	.has-border {
		border-width: 1px;
	}

	.has-background {
		&.neutral {
			background-color: var(--clr-bg-alt);
		}

		&.error {
			background-color: var(--clr-theme-err-container);
		}

		&.pop {
			background-color: var(--clr-theme-pop-container);
		}

		&.warn {
			background-color: var(--clr-theme-warn-container);
		}

		&.success {
			background-color: var(--clr-theme-succ-container);
		}
	}

	/* rendered markdown requires global */
	:global(.info-message__text a) {
		cursor: pointer;
		text-decoration: underline;
		word-break: break-all; /* allow long links to wrap */
	}
	:global(.info-message__text p:not(:last-child)) {
		margin-bottom: var(--size-10);
	}
	:global(.info-message__text ul) {
		list-style-type: circle;
		padding: 0 0 0 var(--size-16);
	}
</style>
