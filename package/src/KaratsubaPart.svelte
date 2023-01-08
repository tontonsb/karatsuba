<script>
	import ClassicPart from './ClassicPart.svelte'

	export let x
	export let y
	export let cutoff

	export let result = 0

	// Link to this component
	export let link = null
	// To generate unique links from this component.
	const nonce = Math.random().toString(36).substring(2)

	let z0, z1, z2

	$: xString = x.toString()
	$: yString = y.toString()

	$: m = Math.floor(Math.min(xString.length, yString.length) / 2)

	$: xSplit = split(xString, m)
	$: ySplit = split(yString, m)

	$: result = (x > cutoff && y > cutoff) ? z2 * Math.pow(10, 2 * m) + (z1 - z2 - z0) * Math.pow(10, m) + z0 : x * y

	$: z0calc = display(xSplit.lower, ySplit.lower)
	$: z1calc = display(
		add(xSplit.lower, xSplit.upper),
		add(ySplit.lower, ySplit.upper),
	)
	$: z2calc = display(xSplit.upper, ySplit.upper)

	function split(str, position) {
		return {
			upper: parseInt(str.substring(0, str.length-position)) || 0,
			lower: parseInt(str.substr(-position)) || 0,
		}
	}

	function add(a, b) {
		return a.toString() + ' + ' + b.toString()
	}

	function display(a, b) {
		a = a.toString()
		b = b.toString()

		return `${a} * ${b}`
	}
</script>

<!-- We only display if we need to do the algo -->
{#if x >= cutoff && y >= cutoff}
<pre id={link} >
{x}
* {y}
-----------------------
<span class=comment>z0 = <a href="#z0-{nonce}">{z0calc}</a> = </span>{z0}
<span class=comment>z1 = <a href="#z1-{nonce}">{z1calc}</a> - z0 - z2 = </span>{z1 - z0 - z2}{' '.repeat(m)}
<span class=comment>z2 = <a href="#z2-{nonce}">{z2calc}</a> = </span>{z2}{' '.repeat(2 * m)}
-----------------------
{result}
</pre>

<svelte:self {cutoff} x={xSplit.lower} y={ySplit.lower} bind:result={z0} link="z0-{nonce}" />
<svelte:self {cutoff} x={xSplit.lower + xSplit.upper} y={ySplit.lower + ySplit.upper} bind:result={z1} link="z1-{nonce}" />
<svelte:self {cutoff} x={xSplit.upper} y={ySplit.upper} bind:result={z2} link="z2-{nonce}" />
{:else}
<ClassicPart {x} {y} {link} />
{/if}

<style>
pre {
	text-align: right;
	font-family: monospace;
	white-space: pre;
}
</style>
