<script>
	export let x
	export let y

	$: digitsX = x.toString().split('').reverse()
	$: digitsY = y.toString().split('').reverse()
	
	$: lenX = digitsX.length
	$: lenY = digitsY.length
	$: length = Math.max(lenX, lenY)

	$: products = digitsX.map((dx, i) => digitsY.map((dy, j) => ({
		x: dx,
		y: dy,
		padding: i + j,
		product: dx * dy,
	}))).flat()
</script>

<pre>
{x}
* {y}
-----------------------
{#each products as p}
<span class=counter/><span class=comment>{p.x}*{p.y} = </span> {p.product}{' '.repeat(p.padding)}
{/each}<!-- prevent line break after last item
-->-----------------------
{products.reduce((sum, p) => sum + p.product * Math.pow(10, p.padding), 0)}
</pre>
