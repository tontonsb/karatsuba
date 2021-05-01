<script>
	export let x
	export let y
	
	export let result = 0

	let z0, z1, z2

	$: xString = x.toString()
	$: yString = y.toString()

	$: m = Math.floor(Math.min(xString.length, yString.length) / 2)

	$: xSplit = split(xString, m)
	$: ySplit = split(yString, m)

	$: result = (x > 10 && y > 10) ? z2 * Math.pow(10, 2 * m) + (z1 - z2 - z0) * Math.pow(10, m) + z0 : x * y

	$: z0calc = display(xSplit.lower, ySplit.lower)
	$: z1calc = display(
		add(xSplit.lower, xSplit.upper), 
		add(ySplit.lower, ySplit.upper), 
		xSplit.lower + xSplit.upper < 10 || ySplit.lower + ySplit.upper < 10
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

	function display(a, b, short) {
		a = a.toString()
		b = b.toString()

		if ('undefined' === typeof short)
			short = a.length <= 1 || b.length <= 1

		if (short)
			return a + ' * ' + b
		
		return `karatsuba(${a}, ${b})`
	}
</script>

<!-- We only display if we need to do the algo -->
{#if x >= 10 && y >= 10}
<pre>
{x}
* {y}
-----------------------
<span class=comment>z0 = {z0calc} = </span>{z0}
<span class=comment>{z1calc} - z0 - z2 = </span>{z1 - z0 - z2}{' '.repeat(m)}
<span class=comment>z2 = {z2calc} = </span>{z2}{' '.repeat(2 * m)}
-----------------------
{result}
</pre>

<svelte:self x={xSplit.lower} y={ySplit.lower} bind:result={z0} />
<svelte:self x={xSplit.lower + xSplit.upper} y={ySplit.lower + ySplit.upper} bind:result={z1} />
<svelte:self x={xSplit.upper} y={ySplit.upper} bind:result={z2} />
{/if}

