<script>
	export let x
	export let y
	export let link = null

	export let result = 0n

	$: digitsX = x.toString().split('').reverse()
	$: digitsY = y.toString().split('').reverse()

	$: products = digitsX.map((dx, i) => digitsY.map((dy, j) => ({
		x: dx,
		y: dy,
		padding: i + j,
		product: BigInt(dx * dy),
		value: BigInt(dx * dy) * (10n ** BigInt(i+j))
	}))).flat()

	$: result = products.reduce((sum, p) => sum + p.value, 0n)
</script>

<pre id={link} >
{x}
* {y}
-----------------------
{#each products as p}
<span class=counter/><span class=comment>{p.x}*{p.y} = </span> {p.product}{' '.repeat(p.padding)}
{/each}<!-- prevent line break after last item
-->-----------------------
{result}
</pre>

<style>
pre {
	text-align: right;
	font-family: monospace;
	white-space: pre;
	position: relative;
}

.counter::before {
	position: absolute;
	left: 0;
	transform: translateX(calc(2em - 100%));
	counter-increment: karatsuba-line;
	content: counter(karatsuba-line);
}
</style>
