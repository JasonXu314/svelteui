<script lang="ts">
	import useStyles from './Badge.styles';
	import { createEventForwarder, useActions } from '$lib/internal';
	import { get_current_component } from 'svelte/internal';
	import Box from '../Box/Box.svelte';
	import type { BadgeProps as $$BadgeProps } from './Badge.styles';

	interface $$Props extends $$BadgeProps {}

	export let use: $$Props['use'] = [],
		element: $$Props['element'] = undefined,
		className: $$Props['className'] = '',
		override: $$Props['override'] = {},
		color: $$Props['color'] = 'blue',
		variant: $$Props['variant'] = 'light',
		gradient: $$Props['gradient'] = { from: 'blue', to: 'cyan', deg: 45 },
		size: $$Props['size'] = 'md',
		radius: $$Props['radius'] = 'xl',
		fullWidth: $$Props['fullWidth'] = false;
	export { className as class };

	/** An action that forwards inner dom node events from parent component */
	const forwardEvents = createEventForwarder(get_current_component());

	$: ({ cx, classes } = useStyles(
		{
			color,
			fullWidth,
			size,
			radius,
			gradientDeg: gradient.deg,
			gradientFrom: gradient.from,
			gradientTo: gradient.to
		},
		{ override }
	));
</script>

<!--
@component
**UNSTABLE:** new API, yet to be vetted.

Display badge, pill or tag

@see https://svelteui.org/core/badge
@example
    ```svelte
    <Badge>Badge</Badge>
	<Badge variant="gradient" gradient={{ from: 'indigo', to: 'cyan' }}>Indigo cyan</Badge>
	<Box css={{ width: 120 }}>
		<Badge variant="filled" fullWidth>Badge with overflow</Badge>
	</Box>
    ```
-->
<Box
	use={[forwardEvents, [useActions, use]]}
	bind:element
	class={cx(className, variant, classes.root)}
	{...$$restProps}
>
	{#if $$slots.leftSection}
		<span class={classes.leftSection}>
			<slot name="leftSection" />
		</span>
	{/if}
	<span class={classes.inner}><slot /></span>
	{#if $$slots.rightSection}
		<span class={classes.rightSection}>
			<slot name="rightSection" />
		</span>
	{/if}
</Box>
