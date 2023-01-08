<script>
	import ClassicPart from './ClassicPart.svelte'

	export let x
	export let y
	export let cutoff

	export let result = 0n

	// Link to this component
	export let link = null
	// To generate unique links from this component.
	const nonce = Math.random().toString(36).substring(2)

	// Capturing results of Karatsuba sub-steps
	let z0, z1, z2
	// Capturing the result of a classic sub-step
	let classic

	$: usingKaratsuba = x >= cutoff && y >= cutoff

	$: xString = x.toString()
	$: yString = y.toString()

	$: m = Math.floor(Math.min(xString.length, yString.length) / 2)

	$: xSplit = split(xString, m)
	$: ySplit = split(yString, m)

	// We have to do the sub-checks because the vars are only calculated later
	// in the sub-components.
	$: result = usingKaratsuba
		? (
			z0 && z1 && z2
			? z2 * (10n ** BigInt(2*m)) + (z1 - z2 - z0) * (10n ** BigInt(m)) + z0
			: 0n
		)
		: (classic ?? 0n)

	$: z0calc = display(xSplit.lower, ySplit.lower)
	$: z1calc = display(
		add(xSplit.lower, xSplit.upper),
		add(ySplit.lower, ySplit.upper),
	)
	$: z2calc = display(xSplit.upper, ySplit.upper)

	function split(str, position) {
		return {
			upper: BigInt(parseInt(str.substring(0, str.length-position))) || 0n,
			lower: BigInt(parseInt(str.substr(-position))) || 0n,
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
{#if usingKaratsuba}
<pre id={link} >
{x}
* {y}
-----------------------
<span class=comment>z0 = <a href="#z0-{nonce}">{z0calc}</a> = </span>{z0}
<span class=comment>z1 = <a href="#z1-{nonce}">{z1calc}</a> - z0 - z2 = </span>{z1 - z0 - z2}{' '.repeat(m)}
<span class=comment>z2 = <a href="#z2-{nonce}">{z2calc}</a> = </span>{z2}{' '.repeat(2 * m)}
-----------------------
<span class="comment">z2*10^(2m) + z1*10^m + z0 = </span>{result}
</pre>

<svelte:self {cutoff} x={xSplit.lower} y={ySplit.lower} bind:result={z0} link="z0-{nonce}" />
<svelte:self {cutoff} x={xSplit.lower + xSplit.upper} y={ySplit.lower + ySplit.upper} bind:result={z1} link="z1-{nonce}" />
<svelte:self {cutoff} x={xSplit.upper} y={ySplit.upper} bind:result={z2} link="z2-{nonce}" />
{:else}
<ClassicPart {x} {y} {link} bind:result={classic} />
{/if}

<style>
pre {
	text-align: right;
	font-family: monospace;
	white-space: pre;
}
</style>
